If you get the following errors while trying to push some data to a remote using git, the tips below might give you a solution.

  Push failed
  fatal: The remote end hung up unexpectedly
  fatal: The remote end hung up unexpectedly
  error: RPC failed; result=56, HTTP code = 0
  
I had this error because I was trying to push a CRM to phpcloud.com, apperantly the default git buffer size was too small.

Add the following settings to your git client config file:

Windows
  [http] 
  postBuffer = 524288000 

Linux:
  git config http.postBuffer 524288000
  
If you have no idea where the 


References:

https://getsatisfaction.com/zend_technologies/topics/cannot_open_git_upload_pack
http://stackoverflow.com/questions/2114111/where-does-git-config-global-get-written-to
