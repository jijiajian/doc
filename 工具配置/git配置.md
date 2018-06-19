
git config --global user.name "J"  
git config --global user.email 12345@xxx.com  
ssh-keygen -t rsa -C "12345@xxx.com"  

## 关联本地项目
新建README.md文件
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:tripleJ1995/java_pick_up.git
git push -u origin master


## 克隆速度快
git clone xxxxx.git --depth=1

git fetch --unshallow