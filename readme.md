From the following transcriptions write the problem statement and the solution description. 

00:02
So, the problem statement that we have currently is, we're opening too many support tickets for. Getting information for etland and its services so? Uh. The atland secure agent runs on k3s, which is a version of kubernetes in kubernetes offers the remote API access. Um. Remote it. The remote Epi access allows us to view things like pods and deployments and stateful sets and stuff like that.

00:43
Um, by calling restful endpoints. In order to? Lock that down for k3s. They have this thing called arbeck or roll-based access control. In order to? Um, give read-only access to users. We would have to enable our back and that's done through a command line parameter on k3s. Once we have that enabled.

01:15
Um, we're able to upload, manifest to Define it, and I have defined a manifest. Uh, for me, the only access that we can give out to SRE and application developers. Um, using certificates in specifying the username in the config files for the desktops of all the people involved. So, the first step that you have to do in order to get this?

01:46
Uh, switched on. Is, we went into the k3s install. Shell script, and there are. Eight lines that? Allow us to add the parameters TLS and which is going to be the name of the host that's we're remotely connecting to. So for, um? Uat, we would. We would specify like the uat server for prod and Dr.

02:18
We would specify it there. Uh, the next thing that we would have to do is to allow. The two parameters of? Anonymous auth. And we would also have to enable API or excuse me authorization mode. The important one for authorization mode is artbeck. Once this is turned on for the servers, we would reboot the servers to start k3s with the new parameters, and that'll get us to the point where we can put in our manifests.

02:55
Uh, for now, I had named it SRE read only. Um, just because? That's the main group that will be using this on a regular basis. In here for this SRE rate-only file. Uh, you specify the namespace that you want the group to have access to. And you would, uh, you give the name of the policy.

03:23
So, for each namespace. We would have this script. Which is a roll fragment, and then it would have a corresponding roll binding fragment. That sets all of the other. Um, like it sets the subject and the role reference. These things running pairwise in the server essentially just runs. It is essentially just does the setup.

03:52
The last thing that we need, as far as server setup setup goes, is we would have to make sure that there is a key gen or excuse me, a public key and a private key. Defined for the credentials of the group that is going to be getting access in order to limit it.

04:12
There is a. Large default generated certificate system-wide for k3s, and then we would need more targeted ones. For, for these purposes here. This. This key has been generated. You're going to download this key onto each of the different workstations. That are going to be connecting to it, and then you're going to have to modify the configuration file.

04:44
For their Cube CTL command. Uh, in order to get the specifics, we'll have to put this up. Where it's available for all the corresponding parties to receive it. Uh, I have here the config file. And the way the way it is structured is you have a cluster section. The Clusters are going to be the remote server that you're connecting to, and it's going to require a certificate in order to open the connection.

05:21
After, which is the context? Which is going to be a connector like the connective tissue between the cluster and the user. And the context is essentially cluster and then user. And then you give it a need to be able to identify it. The last segment is going to be the user's array, and this is where you're going to put in the.

05:49
Client key that you generated. And the certificate for the server that you're connecting to. And that'll give you the the name fragment, and then you just give it an identifiable name. That will be tied together with the cluster up in contexts. Um, what I would recommend doing is having vs code.

06:14
For a GUI to be able to remotely connect instead of just using the kube CTL commands. And there. The extension is available from Microsoft, and it's just called kubernetes. Once you're once you have this component installed, it will have an icon on the left hand side. Called kubernetes. If you click on the triple dots next to the clusters header.

06:43
It will give you an option to do. An existing cluster, create a cluster or set Cube config. I would just say, set Cube config and then browse to it in your, uh, in your command window here. It will show up on the left hand side. You can click an arrow to expand it if you go to name spaces.

07:06
It'll list off the name spaces that are available on that cluster. You can right click on it. And you can click on use namespace and that will set it as your default context for all the other things underneath it. Um, let me do this free Arco uat. And then, if you go to workloads and deployments.

07:35
It will show the list of deployments. There are also options for stateful sets. Demon sits. Jobs. Brian jobs. In pots. If you see a pod with a red dot, that means it is stopped. If you see one with a green, that means it's currently running. If you see an orange, it is pending.

08:03
And if it is black, there was an error.

08:09
From there. You can click on one of the, um, there's three icons on the right. There is describe there's logs, and there's terminal terminal lets you exec into the Pod. Loves. If you click on that, it will bring up its own window. And you can click on run. And it will allow you to view the logs for whatever pod you have selected.

08:37
For. You know for that pot? Uh, there will also be an option that runs more than one container. You can click and drop down to pick which container.

08:56
That's. It isn't super fancy, but it is definitely powerful enough to get you into k3s and look at the current operations that that are going on for it. Okay, do you want to talk about the three four components that at this whole solution is made up of and which one goes where?

09:19
For the. K3s. It updates the k3s rootless service file, one of the the components. The next component is the. Necessary. Read only yaml. This is something that we do once per repave. And it's a multi-document file. It will apply more than one manifest, so if you see here, we've got the namespace Argo sandbox.

10:01
And we got Argo experimental. Ah, Argo, uat. And then, for the roll bindings, it's the same thing sandbox. Experimental UVT. So, there's this document here. There's also the. Private key that will be generated, so there is, um. Let's see where there's mine out. There is SRE CRT. And going along with that is.

10:41
Uh, s, r, e, t. Which is here.

10:52
Let's see. I'm switches here. SRE, he is here. And then SRE cert. Is here. And then the last part is the config, and the config is going to live in users. User ID dot koob config. Uh, what are the changes that are being done to the secure agent? Um, big bucket repo for enabling this capability.

11:30
Is done in k3s install. Lines 154 and 156. This is basically enabling the r back on the kubernetes. That's correct. The arbeck is going.

11:49
Yes, the arvec is going from. Line 156, column 120.

12:00
And line 154. Same thing roles for the, uh, our back are defined, in which place again, just. I think we already covered it, but just from, uh, they're going under config. Manifests. And there is one called SRE Dash read-only.yml. And this would be one file one ml file for each role that we would want to configure.

12:31
There are multiple rows rolls r-o-l-e-s in one file. Okay? Now, when it so if and when we go to production? We would have, uh, the namespace might just be Argo, but we would still have one role here. And then one row binding. Down here. For each namespace. Okay. So, if we add more namespaces, we will need to add more of these documents.

13:05
These roll and roll binding documents? Fine, uh, do you want to also separate these? Those. Environment folders. Environment conflicts like, you have the environment conflicts. Ut sandbox product TR and so forth, right? So, if the namespace doesn't exist, it doesn't give us it. It'll throw a warning, but it will not stop it from running so.

13:39
It is, you know? It does a soft failover. So we can actually have. There's many name spaces as we want in here. In one file, and if the environment doesn't have the namespace, then it will simply continue on to the next one. So in theory, we could say, uat, sandbox experimental prod in Dr can all live in this one file, and if we apply it in the uat environment.

14:10
It will only apply to the namespaces that exist, and if we apply it in prod, it won't apply any of the. Uat environment namespaces. Okay, so then I think we should just populate. All environment sections in this one file then. Correct. Okay. And do one single deployment in a regular manner.

14:34
Rent. Okay. Lastly, I think last thing to cover would be. Explore one of the failures in. Dev shell and? Then explore the same thing in. Uh, on the vs code. Let me open up a deaf shell, and we are. We are right now connected to uat, right? You can see as of right now.

15:00
I think the snowflake is failing and see how it helps that situation of. Investigating snowflake.

15:12
Okay.

16:27
And you said sandbox? Um, here it is.

17:30
So do it in the dev shell first and? The get part center for u rating or u80? And then let's explore the same thing here.

18:07
All right.

18:13
So? From. Can bevshell slash vs code terminal green Cube CTL logs Etlin snowflake and then the identifier fort? Um, once I hit enter because I had already set my default context. Free UAT over here on the left. Um. My deadlocks for? Spun out the logs. Where we can see right here, says connect timed out.

18:47
From the Java URL connector. And the same thing should be showing up here. And it is not because it's a different container so. This is where I was talking about with the drop down. We have fetch credentials in it, worn in Maine. And I think if we do the same thing here.

19:16
Yeah. So, if I type init?

19:24
Uh, Ming. Let's see. Yeah, in Maine. So I can get clear and then. And there's the different. For me.

19:43
Okay. All right. And of course, um, this kind of stuff can be combined with bash scripts or Powershell scripts for the kube CTL commands. So, if somebody wants to break like a super fancy, um? Batch script. That takes all of the running pods. Takes all of the lugs, exports the mount to text files, and then Zips them up.

20:13
And then you have like a. Package that you can send off to atland that they can unzip and take a look at for all the different log messages. Um, they're more than welcome to do it. I recommend using vs code. Because it's got a very easy going to navigate.

20:33
And um? You can. Use this to show other people through through. Zoom and whatnot. Okay, all right. Thank you so much, Steven. That is all for now and!