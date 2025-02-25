```

$$$$$$$\                      $$\                                 $$$$$$$\  $$$$$$$$\       $$$$$$$\             $$$$$$\  
$$  __$$\                     $$ |                                $$  __$$\ $$  _____|      $$  __$$\           $$  __$$\ 
$$ |  $$ | $$$$$$\   $$$$$$$\ $$ |  $$\  $$$$$$\   $$$$$$\        $$ |  $$ |$$ |            $$ |  $$ | $$$$$$\  $$ /  \__|
$$ |  $$ |$$  __$$\ $$  _____|$$ | $$  |$$  __$$\ $$  __$$\       $$$$$$$  |$$$$$\          $$$$$$$  |$$  __$$\ $$ |      
$$ |  $$ |$$ /  $$ |$$ /      $$$$$$  / $$$$$$$$ |$$ |  \__|      $$  ____/ $$  __|         $$  ____/ $$ /  $$ |$$ |      
$$ |  $$ |$$ |  $$ |$$ |      $$  _$$<  $$   ____|$$ |            $$ |      $$ |            $$ |      $$ |  $$ |$$ |  $$\ 
$$$$$$$  |\$$$$$$  |\$$$$$$$\ $$ | \$$\ \$$$$$$$\ $$ |            $$ |      $$$$$$$$\       $$ |      \$$$$$$  |\$$$$$$  |
\_______/  \______/  \_______|\__|  \__| \_______|\__|            \__|      \________|      \__|       \______/  \______/  
```

# Legal Disclaimer
This script and instructions are provided for educational and authorized testing purposes only. Use of this method on systems you do not own, or do not have explicit permission to test, may be illegal. The author and contributors assume no liability for actions taken based on this material.

# Docker Privilege Escalation PoC
This PoC demonstrates how a non-privileged Linux user with membership in the "docker" group (but WITHOUT sudo access) can escalate privileges by leveraging Docker's ability to run containers with elevated privileges and mount the host filesystem. By mounting your home directory and copying a SUID-root binary onto the host, an attacker can spawn a root shell.

I learned this while doing the [GoodGames](https://www.hackthebox.com/machines/goodgames) HackTheBox machine. I've always known there was a warning on why adding a user to the Docker group was potentially dangerous, but didn't understand why till doing this machine. Super cool, nothing unique or special. I just wanted to showcase it in a script for others to learn/use. 


# USAGE
1. Ensure Docker is installed and you have access via the docker group:
   - Run 'groups' to confirm 'docker' is listed.
2. Copy the script files (exploit.sh and wait.sh) to a directory in your home folder.
3. Make exploit.sh executable:
      `chmod +x exploit.sh`
4. Run exploit.sh:
      `./exploit.sh`
5. After a brief delay, the script copies `/bin/bash` -> `~/root-me-pls`, sets the SUID bit in a container, and then uses it locally to spawn a root shell.
6. When you exit the root shell, the script cleans up all artifacts, 
   including the container and temporary files.


# SYSTEM REQUIREMENTS & LIMITATIONS
- Linux host where you have Docker installed and can run containers 
  without sudo (i.e., you're in the docker group).
- Docker daemon configured to allow privileged containers 
  (or pass --privileged when running the container).
- SUID bits may be disallowed in some Docker configurations. In that case, 
  adjustments or privileges might be needed for the mount to respect SUID.


# REFERENCES

- Docker Security (Official docs): 
  https://docs.docker.com/engine/security/security/
- Hack The Box and TheCyberGeek:
  https://www.hackthebox.com/machines/goodgames


