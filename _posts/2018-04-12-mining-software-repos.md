---
layout: post
title: Mining Software Repositories to learn a new framework
excerpt_separator: <!--more-->
---

So here is the idea: a framework is getting hype and you want to learn it. You consult the documentation, read a few short blog posts about how to do X in the cool framework Y.  
But what if you can get some insights on how to _really_ use the framework?  
This is where the idea the script in this blog post came from.  
<!--more-->
For a framework that gained some attention there would (likely) be some GitHub projects that utilize it and built on top of it. So why not see how those projects are actually using the framework?  
Git is not only a tool to make you feel safe that you got your past versions, or that you can blame someone for bad code. But it can also be a knowledge mine if you dig in it.  
The script I made for this purpose is actually simple, it gets the git log of a repository, then gets the changeset for each commit.  

How to use the script:
First you’ll have to clone a repository to your local machine, then run the script:
```bash
python pythonplayground.py
```
It will ask for the path of the git repository, paste it in.  
The output will be an html file that shows some information about the commits (commit sha, author, date, subject), and the files `A`dded or `M`odified in the commit.  

The following html is the output of the script running on cloned [TypiCMS](https://github.com/TypiCMS/Base) repository:  
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: start;">
      <th></th>
      <th>sha</th>
      <th>author</th>
      <th>files_in_commit</th>
      <th>subject</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>60</th>
      <td>8c0fc2b</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;public/js/admin.js</td>
      <td>admin.js compiled</td>
      <td>Fri Sep 1 14:09:02 2017 +0200</td>
    </tr>
    <tr>
      <th>61</th>
      <td>da0170c</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;public/js/admin.js</div><div>M&nbsp;&nbsp;public/js/public.js</td>
      <td>js compiled</td>
      <td>Thu Aug 31 23:59:52 2017 +0200</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2164cf6</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;public/components/ckeditor/ckeditor.js</div><div>M&nbsp;&nbsp;public/components/ckeditor/config.js</div><div>M&nbsp;&nbsp;public/components/ckeditor/plugins/justify/plugin.js</div><div>M&nbsp;&nbsp;public/components/ckeditor/plugins/scayt/dialogs/options.js</div><div>M&nbsp;&nbsp;public/components/ckeditor/plugins/tableselection/styles/tableselection.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/plugins/widget/plugin.js</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/kama/editor.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/kama/editor_ie.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/kama/editor_ie7.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/kama/editor_ie8.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/kama/editor_iequirks.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono-lisa/editor.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono-lisa/editor_gecko.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono-lisa/editor_ie.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono-lisa/editor_ie8.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono-lisa/editor_iequirks.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor_gecko.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor_ie.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor_ie7.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor_ie8.css</div><div>M&nbsp;&nbsp;public/components/ckeditor/skins/moono/editor_iequirks.css</td>
      <td>ckeditor</td>
      <td>Thu Aug 31 23:59:45 2017 +0200</td>
    </tr>
    <tr>
      <th>63</th>
      <td>81a68af</td>
      <td>Samuel De Backer</td>
      <td></td>
      <td>old config file removed</td>
      <td>Thu Aug 31 23:42:43 2017 +0200</td>
    </tr>
    <tr>
      <th>64</th>
      <td>a1fda7d</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.lock</td>
      <td>cu</td>
      <td>Thu Aug 31 23:36:30 2017 +0200</td>
    </tr>
    <tr>
      <th>65</th>
      <td>8f6c3fd</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.json</td>
      <td>Packages versions in composer.json</td>
      <td>Thu Aug 31 23:13:14 2017 +0200</td>
    </tr>
    <tr>
      <th>66</th>
      <td>45e71d1</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;CHANGELOG.md</td>
      <td>changelog</td>
      <td>Thu Aug 31 23:12:43 2017 +0200</td>
    </tr>
    <tr>
      <th>67</th>
      <td>ead97e1</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.json</div><div>M&nbsp;&nbsp;composer.lock</div><div>M&nbsp;&nbsp;config/app.php</div><div>A&nbsp;&nbsp;config/translatable.php</td>
      <td>cu</td>
      <td>Thu Aug 31 18:51:54 2017 +0200</td>
    </tr>
    <tr>
      <th>68</th>
      <td>f8c223b</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.lock</td>
      <td>cu</td>
      <td>Thu Aug 24 12:06:31 2017 +0200</td>
    </tr>
    <tr>
      <th>69</th>
      <td>026e98f</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.lock</div><div>M&nbsp;&nbsp;public/css/admin.css</div><div>M&nbsp;&nbsp;public/css/public.css</td>
      <td>css compiled</td>
      <td>Tue Aug 22 12:46:10 2017 +0200</td>
    </tr>
    <tr>
      <th>70</th>
      <td>435d2d1</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;public/css/admin.css</div><div>M&nbsp;&nbsp;public/css/public.css</div><div>M&nbsp;&nbsp;public/js/admin.js</div><div>M&nbsp;&nbsp;public/js/public.js</div><div>M&nbsp;&nbsp;public/mix-manifest.json</td>
      <td>assets</td>
      <td>Tue Aug 22 12:28:10 2017 +0200</td>
    </tr>
    <tr>
      <th>71</th>
      <td>6f4a79d</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;composer.lock</td>
      <td>cu</td>
      <td>Tue Aug 22 12:28:05 2017 +0200</td>
    </tr>
    <tr>
      <th>72</th>
      <td>39adc58</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;public/css/admin.css</td>
      <td>css compiled</td>
      <td>Fri Aug 18 17:43:22 2017 +0200</td>
    </tr>
    <tr>
      <th>73</th>
      <td>0f110ab</td>
      <td>ldaril</td>
      <td><div>M&nbsp;&nbsp;resources/lang/es.json</td>
      <td>Delete unnecessary space</td>
      <td>Fri Aug 18 14:06:25 2017 +0200</td>
    </tr>
    <tr>
      <th>74</th>
      <td>7bf585a</td>
      <td>ldaril</td>
      <td><div>M&nbsp;&nbsp;resources/lang/fr.json</td>
      <td>Delete unnecessary space</td>
      <td>Fri Aug 18 14:05:21 2017 +0200</td>
    </tr>
    <tr>
      <th>75</th>
      <td>338efef</td>
      <td>Samuel De Backer</td>
      <td><div>M&nbsp;&nbsp;resources/lang/es.json</div><div>M&nbsp;&nbsp;resources/lang/fr.json</td>
      <td>Non-breaking space removed</td>
      <td>Fri Aug 18 11:37:17 2017 +0200</td>
    </tr>
  </tbody>
</table>


My initial intention was to visualize the output with D3.js, but pandas [DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) makes it easy to write an html output of the DataFrame you built. So that’s what I ended up doing in the script for now.  

The script is well documented, and should be easy to modify.  
https://github.com/moafak/MSR/blob/22d7a1019280b215e4e32378160007179d20281a/gitplayground.py#L1

The python libraries used are:  
* [GitPython](https://github.com/gitpython-developers/GitPython) to interact with git  
* [Pandas](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)  for dataframes  


Thanks for reading.  
Cheers.  

