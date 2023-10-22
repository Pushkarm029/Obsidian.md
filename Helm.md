- Bundle of YAML Files
- Create your own helm charts with Helm
- Push them to Helm Repository
- Download and use existing one

### Sharing

Best Feature to download pre configured charts.

### Templating Engine
- imagine, you have multiple pods in microservice with most of the YAML same with some values changed,
- solution -> create a values.yaml like![[Pasted image 20231018115548.png]]
- then import it in required deployment.yaml ![[Pasted image 20231018115526.png]]
- Practical for CI/CD
- can configure yaml for dev,staging,prod easily.


### Helm Chart Structure
![[Pasted image 20231018115718.png]]
- Template files will be filled with the values from values.yaml .
```bash
helm install <chartname>
```

- value injection into template file using cli
```bash
helm install --set version=2.0.0
```

### Release Management

#### Helm V2
![[Pasted image 20231018120000.png]]
![[Pasted image 20231018120016.png]]
- handling rollbacks

### Downsides
![[Pasted image 20231018120049.png]]

