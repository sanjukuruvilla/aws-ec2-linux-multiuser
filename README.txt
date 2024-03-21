## Objective

The objective of this project is to create an EC2 Red Hat Linux instance on AWS with multiple users (Mike, Jack, and Jill), each having their dedicated working space and restricted access to other users' folders or files. Users should use the latest SSH keypair authentication method to log in to the system.

## Table of Contents

1. [Objective](#objective)
2. [Setup Instructions](#setup-instructions)
3. [Additional Documentation](#additional-documentation)
4. [Usage](#usage)
5. [Contributing](#contributing)
6. [License](#license)

## Setup Instructions

1. **Launch EC2 Instance**:
   - Launch an Amazon Linux EC2 instance in AWS.

2. **Create Linux Users**:
   - Create three users in Linux with respective home directories:
     ```
     sudo useradd Mike -m -d /home/Mike -s /bin/bash
     sudo useradd Jack -m -d /home/Jack -s /bin/bash
     sudo useradd Jill -m -d /home/Jill -s /bin/bash
     ```

3. **IAM Configuration**:
   - Create IAM users and provide full access to the EC2 instance.

4. **SSH Keypair Generation**:
   - Generate SSH key pairs for each user:
     ```
     mkdir -p ~Mike && ssh-keygen -t rsa -f ~/Mike/Mike
     mkdir -p ~Jack && ssh-keygen -t rsa -f ~/Jack/Jack
     mkdir -p ~Jill && ssh-keygen -t rsa -f ~/Jill/Jill
     ```

5. **SSH Configuration**:
   - Configure SSH to allow public key authentication:
     - Edit the /etc/ssh/sshd_config file:
       ```
       PasswordAuthentication no
       PubkeyAuthentication yes
       ```
     - Restart the SSH service:
       ```
       sudo systemctl restart sshd
       ```

6. **Authorize SSH Access**:
   - Create .ssh folders in each user's home directory and add their public keys to the authorized_keys file:
     ```
     sudo -u Mike mkdir /home/Mike/.ssh
     sudo -u Mike echo "contents_of_MikeKey.pub" > /home/Mike/.ssh/authorized_keys
     # Repeat for Jack and Jill
     ```

7. **Grant Sudo Privileges**:
   - Add Mike to the wheel group and configure sudoers file:
     ```
     usermod -aG wheel Mike
     visudo
     # Uncomment the line '%wheel ALL=(ALL) NOPASSWD: ALL'
     ```

## Additional Documentation

For more detailed instructions and best practices, please refer to the [documentation folder](documentation/) in this repository. The documentation includes PDF files with step-by-step guides and configuration details.

## Usage

Follow the provided instructions to set up the EC2 Linux instance with multiple users and SSH keypair authentication.

## Contributing

Contributions to this repository are welcome! If you find issues or have suggestions for improvements, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).
