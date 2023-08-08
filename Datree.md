![[Pasted image 20230729204308.png]]

- Catch Misconfigurations in the Development Phase
- Process occurs in these steps : -
	- YAML Validation
	- K8s Schema Validation
	- Policy Check (Contains 30 Policy)
- Previous Results can be seen in Web Dashboard.
- Policy can be edited, enabled and disabled in web dashboard.
- Can Customize the Error Message
- You Can Collaborate via Enabling Policy as Code Option. (Will download a policies.yaml)
- After downloading policies.yaml , you can make changes locally by commenting the policy that you want to disable, Then you can publish by running this code in Docker

```bash
	datree publish policies.yaml
```

- All this happen with a specific token. you can find your here.
```
	cat ~/.datree/config.yaml
```
- Using Datree on New Device. You can change token with this

1.  ignore this ("")
```
	export DATREE_TOKEN="user's_token"
```
2.  Overwrite This
```
	cat ~/.datree/config.yaml
```
3.  ignore this ("")
```
	datree config set token "user's_token"
```















## Source -> 
<br>
![YT](https://www.youtube.com/watch?v=i-osqorJAxI&list=PL9gnSGHSqcnoqBXdMwUTRod4Gi3eac2Ak&index=9&t=174s)

