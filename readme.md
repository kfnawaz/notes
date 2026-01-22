import os
import sys
import argparse
import csv
from datetime import datetime
from typing import List, Optional
from pyatlan.client.atlan import AtlanClient
from pyatlan.model.assets import Asset, SQL, Database
from pyatlan.model.fluent_search import FluentSearch, CompoundQuery
from pyatlan.model.enums import AtlanDeleteType

DEFAULT_BASE_URL = "https://abc.atlan.com"
DEFAULT_API_KEY = "ey"


def get_value(cli_value: Optional[str],
              env_var: str,
              default_value: Optional[str] = None,
              required: bool = False) -> str:

    value = cli_value or os.getenv(env_var) or default_value
    if required and not value:
        raise SystemExit(
            f"missing required value: provide --{env_var.lower().replace('_', '-')}, "
            f"set {env_var} env var, or set DEFAULT_{env_var} in the script")
    return value


def parse_args():
    parser = argparse.ArgumentParser(
        description="Permanently delete all archived assets",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog=__doc__)

    parser.add_argument("--base-url",
                        type=str,
                        default=None,
                        help="Atlan base URL (or set ATLAN_BASE_URL env var)")
    parser.add_argument("--api-key",
                        type=str,
                        default=None,
                        help="Atlan API key (or set ATLAN_API_KEY env var)")
    parser.add_argument(
        "--database-qn",
        type=str,
        default=None,
        help="Database qualified name (or set DATABASE_QUALIFIED_NAME env var)"
    )
    parser.add_argument(
        "--dry-run",
        action="store_true",
        help="Preview what would be deleted without actually deleting")
    parser.add_argument(
        "--batch-size",
        type=int,
        default=100,
        help="Number of assets to delete per batch (default: 100)")

    return parser.parse_args()


def find_archived_assets(client: AtlanClient, database_qn: str) -> List[Asset]:
    print(f"searching for archived assets for database: {database_qn}")

    search = (FluentSearch.select(include_archived=True).where(
        CompoundQuery.archived_assets()).where(
            SQL.DATABASE_QUALIFIED_NAME.eq(database_qn)).include_on_results(
                Asset.QUALIFIED_NAME.atlan_field_name).include_on_results(
                    Asset.NAME.atlan_field_name).include_on_results(
                        Asset.TYPE_NAME.atlan_field_name).include_on_results(
                            Asset.GUID.atlan_field_name).page_size(1000))

    results = search.execute(client=client)
    assets = list(results)

    print(f"found {len(assets)} archived assets")
    return assets


def find_archived_database(client: AtlanClient,
                           database_qn: str) -> Optional[Database]:
    try:
        search = (FluentSearch.select(include_archived=True).where(
            CompoundQuery.archived_assets()).where(
                Database.QUALIFIED_NAME.eq(database_qn)).include_on_results(
                    Asset.QUALIFIED_NAME.atlan_field_name).include_on_results(
                        Asset.NAME.atlan_field_name).include_on_results(
                            Asset.TYPE_NAME.atlan_field_name).
                  include_on_results(Asset.GUID.atlan_field_name))

        results = search.execute(client=client)
        page = results.current_page()

        if page and len(page) > 0:
            database = page[0]
            if isinstance(database, Database):
                return database

        return None
    except Exception as e:
        print(f"could not search for archived database: {e}")
        return None


def export_assets_to_csv(assets: List[Asset], database_qn: str) -> str:
    if not assets:
        return None

    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    safe_db_name = database_qn.replace("/", "_").replace("\\", "_")
    filename = f"archived_assets_{safe_db_name}_{timestamp}.csv"

    print(f"exporting {len(assets)} asset(s) to CSV: {filename}")

    try:
        with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = [
                'GUID', 'Type Name', 'Name', 'Qualified Name', 'Status'
            ]
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            writer.writeheader()

            for asset in assets:
                writer.writerow({
                    'GUID':
                    asset.guid or '',
                    'Type Name':
                    asset.type_name or '',
                    'Name':
                    asset.name or '',
                    'Qualified Name':
                    asset.qualified_name or '',
                    'Status':
                    getattr(asset, 'status', 'DELETED') or 'DELETED'
                })

        print(f"successfully exported to {filename}")
        return filename
    except Exception as e:
        print(f"failed to export CSV: {e}")
        return None


def purge_archived_assets(client: AtlanClient,
                          assets: List[Asset],
                          batch_size: int = 100,
                          dry_run: bool = False,
                          database_qn: str = None) -> List[Asset]:
    if not assets:
        print("no assets to delete.")
        return []

    if dry_run:
        print("DRY RUN MODE - No assets will be deleted......")
        print(
            f"would permanently delete (purge) {len(assets)} archived assets:\n"
        )
        for asset in assets:
            print(f" - {asset.type_name}: {asset.name} (GUID: {asset.guid})")
        print(f"total: {len(assets)} asset(s)")

        if database_qn:
            csv_file = export_assets_to_csv(assets, database_qn)
            if csv_file:
                print(f"asset list exported to: {csv_file}")
        return assets

    print(
        f"permanently deleting {len(assets)} archived assets in batches of {batch_size}..."
    )

    deleted_count = 0
    failed_count = 0
    deleted_assets = []

    for i in range(0, len(assets), batch_size):
        batch = assets[i:i + batch_size]
        batch_guids = [asset.guid for asset in batch]

        try:
            print(
                f"deleting batch {i // batch_size + 1} ({len(batch)} assets)..."
            )
            response = client.asset.purge_by_guid(
                guid=batch_guids, delete_type=AtlanDeleteType.PURGE)
            deleted = response.assets_deleted(asset_type=Asset)
            deleted_count += len(deleted)
            deleted_assets.extend(deleted)

            print(f"successfully deleted {len(deleted)} assets")

            if len(deleted) < len(batch):
                failed_count += len(batch) - len(deleted)
                print(f"{len(batch) - len(deleted)} assets failed to delete")

        except Exception as e:
            failed_count += len(batch)
            print(f"error deleting batch: {e}")
            print(
                f"failed GUIDs: {', '.join(batch_guids[:5])}{'...' if len(batch_guids) > 5 else ''}"
            )
            import traceback
            if i == 0:  # Show full traceback for first batch
                traceback.print_exc()

    print(f"successfully deleted: {deleted_count} assets")
    if failed_count > 0:
        print(f"failed to delete: {failed_count} assets")
    print(f"total processed: {len(assets)} assets")

    if deleted_assets and database_qn:
        csv_file = export_assets_to_csv(deleted_assets, database_qn)
        if csv_file:
            print(f"deleted assets exported to: {csv_file}")

    return deleted_assets


def main():
    args = parse_args()

    base_url = get_value(args.base_url,
                         "ATLAN_BASE_URL",
                         default_value=DEFAULT_BASE_URL,
                         required=True)
    api_key = get_value(args.api_key,
                        "ATLAN_API_KEY",
                        default_value=DEFAULT_API_KEY,
                        required=True)
    database_qn = get_value(args.database_qn,
                            "DATABASE_QUALIFIED_NAME",
                            required=True)
    dry_run = args.dry_run
    batch_size = args.batch_size

    print(f"Base URL: {base_url}")
    print(f"Database Qualified Name: {database_qn}")
    print(f"Dry Run: {dry_run}")
    print(f"Batch Size: {batch_size}")

    try:
        client = AtlanClient(base_url=base_url, api_key=api_key)
        print("successfully authenticated\n")
    except Exception as e:
        print(f"failed to initialize AtlanClient: {e}")
        sys.exit(1)

    try:
        assets = find_archived_assets(client, database_qn)
    except Exception as e:
        print(f"failed to search for archived assets: {e}")
        sys.exit(1)

    try:
        deleted_assets = purge_archived_assets(client,
                                               assets,
                                               batch_size=batch_size,
                                               dry_run=dry_run,
                                               database_qn=database_qn)
    except Exception as e:
        print(f"failed to delete assets..... {e}")
        import traceback
        traceback.print_exc()
        sys.exit(1)

    print("\nchecking for archived database...")

    try:
        archived_database = find_archived_database(client, database_qn)

        if archived_database:
            print(
                f"found archived database: {archived_database.name} (GUID: {archived_database.guid})"
            )

            if dry_run:
                print(
                    f"[DRY RUN] would delete database: {archived_database.name}"
                )
            else:
                print(f"deleting archived database...")
                try:
                    response = client.asset.purge_by_guid(
                        guid=archived_database.guid,
                        delete_type=AtlanDeleteType.PURGE)

                    deleted_db = response.assets_deleted(asset_type=Database)

                    if deleted_db and len(deleted_db) > 0:
                        print(
                            f"successfully deleted database: {archived_database.name}"
                        )

                        if deleted_assets:
                            all_deleted = deleted_assets + deleted_db
                            csv_file = export_assets_to_csv(
                                all_deleted, database_qn)
                            if csv_file:
                                print(f"updated CSV with database deletion")
                    else:
                        print(f"database deletion returned no deleted assets")
                        print(
                            f"response mutated_entities: {response.mutated_entities is not None}"
                        )
                except Exception as e:
                    print(f"error deleting database: {e}")
                    import traceback
                    traceback.print_exc()
        else:
            print("no archived database found")
    except Exception as e:
        print(f"could not check for archived database: {e}")

    print("completed successfully")


if __name__ == "__main__":
    sys.exit(main())

