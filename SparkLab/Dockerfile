# Use Alpine Linux as the base image
FROM alpine:latest

# Install necessary packages, including JuiceFS dependencies and PostgreSQL
RUN apk add --no-cache curl fuse postgresql

# Get the Latest Verison Tag
RUN JFS_LATEST_TAG=$(curl -s https://api.github.com/repos/juicedata/juicefs/releases/latest | grep 'tag_name' | cut -d '"' -f 4 | tr -d 'v')
# Download and install JuiceFS
RUN curl -s -L "https://github.com/juicedata/juicefs/releases/download/v${JFS_LATEST_TAG}/juicefs-${JFS_LATEST_TAG}-linux-amd64.tar.gz" | tar -xz -C /usr/local/bin

# Setup your environment variables for JuiceFS authentication
ENV JFS_ACCESS_KEY=AKIAYNQKOAY4WYERQZEY
ENV JFS_SECRET_KEY=Jgt5TtkO3D3Me5pwXxJxtpc5IEPLW03B3ZfTEoBn

# Initialize the JuiceFS file system (Replace with your actual configuration)
RUN juicefs auth <your-filesystem-name> --accesskey $JFS_ACCESS_KEY --secretkey $JFS_SECRET_KEY

# Create a directory for mounting JuiceFS
RUN mkdir -p /mnt/jfs

# Initialize the PostgreSQL data directory (to be run inside the mounted JuiceFS directory)
RUN initdb -D /mnt/jfs/data

# Command to mount the S3 bucket using JuiceFS and start PostgreSQL
CMD ["sh", "-c", "juicefs mount <your-filesystem-name> /mnt/jfs && postgres -D /mnt/jfs/data"]
