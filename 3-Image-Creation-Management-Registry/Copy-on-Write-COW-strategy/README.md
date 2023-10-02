
## README: Copy-on-Write (COW) in Docker

Copy-on-Write (COW) is a resource management strategy used by sudo docker to optimize the way it handles storage for containers. This strategy allows sudo docker to save storage space and minimize overhead by sharing common data between containers whenever possible.

### How it works

When a container is created, docker uses a layered file system to store the container's data. Each layer corresponds to an instruction in the Dockerfile, such as adding a file or running a command. These layers are read-only and shared between containers that use the same base image.

With the Copy-on-Write strategy, when a container needs to modify a file from its base image, sudo docker does not make changes to the original shared layer. Instead, it creates a new writable layer on top of the existing layers and makes a copy of the file in this new layer. The container then modifies the copied file in the writable layer, leaving the original shared layer unchanged.

### Benefits

The Copy-on-Write strategy provides several benefits:

- **Storage efficiency**: Since containers share the read-only layers, it reduces the storage space required for multiple containers using the same base image.
- **Faster container startup**: Containers only need to create a writable layer on top of the shared layers, making the startup process faster.
- **Resource isolation**: Containers can modify their own writable layer without affecting other containers, ensuring resource isolation and data consistency.


### Example

To illustrate the Copy-on-Write strategy in Docker, let's use an example with two containers that share a common base image.

First, create a simple Dockerfile for a base image with a text file:

```
FROM ubuntu:20.04

RUN echo "This is the original file in the base image." > /base_file.txt
```

- Build the base image:

```
sudo docker build -t base-image .
```

Now, create and run two containers from the base image:


```

sudo docker run -d --name container1 base-image tail -f /dev/null
sudo docker run -d --name container2 base-image tail -f /dev/null
```

At this point, both containers share the same base image layers, including the /base_file.txt. No extra storage is consumed for the shared file.

- Modify the /base_file.txt in container1:


```

sudo docker exec container1 sh -c "echo 'Modified file in container1.' > /base_file.txt"
```

With the COW strategy, docker creates a new writable layer for container1 and places the modified /base_file.txt in that layer. The original file in the shared base image remains unchanged.

- Verify the contents of /base_file.txt in both containers:


```

sudo docker exec container1 cat /base_file.txt
sudo docker exec container2 cat /base_file.txt
```

You'll see that the file in container1 contains the modified content, while the file in container2 still contains the original content from the base image.

This example demonstrates the Copy-on-Write strategy in Docker, where containers share common data from the base image but create new writable layers for any modifications, ensuring storage efficiency and resource isolation.

### Conclusion

The Copy-on-Write strategy in sudo docker optimizes storage usage, improves container startup time, and maintains resource isolation by sharing common data between containers and only creating copies of modified files when needed. This approach allows for better utilization of resources and a more efficient deployment of containers.

# Disclaimer
<details>

Please note that the entire repository is owned and maintained by [Varun Kumar Manik](https://www.linkedin.com/in/vkmanik/). While every effort has been made to ensure the accuracy and reliability of the information and resources provided in this repository, Varun Kumar Manik takes full responsibility for any errors or inaccuracies that may be present.

Simplilearn is not responsible for the content or materials provided in this repository and disclaims all liability for any issues, misunderstandings, or claims that may arise from the use of the information or materials provided. By using this repository, you acknowledge that Varun Kumar Manik is solely accountable for its content, and you agree to hold Simplilearn harmless from any claims or liabilities that may arise as a result of your use or reliance on the information provided herein.

It is important to understand that this repository contains educational materials for a training course, and users are expected to apply their own judgment and discretion when utilizing the provided resources. Neither Varun Kumar Manik nor Simplilearn can guarantee specific results or outcomes from following the materials in this repository.

</details>

## Connect & Follow

For more info, please connect and follow me:

- Github: [https://github.com/manikcloud](https://github.com/manikcloud)
- LinkedIn: [https://www.linkedin.com/in/vkmanik/](https://www.linkedin.com/in/vkmanik/)
- Email: [varunmanik1@gmail.com](mailto:varunmanik1@gmail.com)
- Facebook: [https://www.facebook.com/cloudvirtualization/](https://www.facebook.com/cloudvirtualization/)
- YouTube: [https://bit.ly/32fknRN](https://bit.ly/32fknRN)
- Twitter: [https://twitter.com/varunkmanik](https://twitter.com/varunkmanik)