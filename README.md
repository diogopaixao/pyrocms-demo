<p class="active">Getting PyroCMS up and running is pretty simple. There are a few things to be aware of when launching PyroCMS on Pagoda Box. After reading this, you should be able to:</p>
<ul class="checked">
	<li>
		<span class="guides-sprite check">&nbsp;</span>Create a local instance of PyroCMS</li>
	<li>
		<span class="guides-sprite check">&nbsp;</span>Take your local instance of PyroCMS and deploy it to Pagoda Box</li>
	<li>
		<span class="guides-sprite check">&nbsp;</span>Create a Pagoda database and access it from your PyroCMS app</li>
	<li>
		<span class="guides-sprite check">&nbsp;</span>Bask in the combined awesomeness of PyroCMS and Pagoda Box</li>
</ul>
<div class="line-divider">&nbsp;</div>
<h2 id="launching-your-pyrocms-app">Launching Your PyroCMS App</h2>
<div class="justify">
  <h4 id="-create-a-local-instance-of-your-app">Create a Local Instance of PyroCMS</h4>
  <p>We&rsquo;re assuming that you&rsquo;ve already done this. If you haven&rsquo;t, follow the <a href="http://www.pyrocms.com/docs" target="_blank">PyroCMS install guide</a>, but rather than uploading to a web server, place the files in your local web server's doc root.</p>
  <h3 class="tag">
	<span class="guides-sprite w-cap">&nbsp;</span><span class="horizontal-guides-sprite white">Important: No Auto-Installers on Pagoda Box</span><span class="guides-sprite w-end-cap">&nbsp;</span></h3>
	<div class="block yellow">
		<p>You are not able to run auto-installers on Pagoda Box. If you use PyroCMS&#39;s auto-installer, run it locally to generate all the necessary configuration files and database tables, then deploy your app to Pagoda Box.</p>
	</div>
	<h4 id="-create-a-github-repo-for-your-app">Create a Git Repo for Your PyroCMS App</h4>
	<p><a href="/customer/portal/articles/202225-setting-up-git">Set up Git</a>, if you haven't already. Then initialize your app as a git repo.</p>
  <h4 id="boxfile-example">Boxfile Example</h4>
  <p>Create a file named &quot;Boxfile&quot; in the base directory of your git repo and paste in the following code snippet. The <a href="/customer/portal/articles/175475">Boxfile</a> is your Pagoda Box config file.</p>
  <h3 class="tag" id="pyrocms-example-box-file">
  <span class="guides-sprite cap">&nbsp;</span><span class="horizontal-guides-sprite title">YAML</span><span class="horizontal-guides-sprite green">PyroCMS Boxfile Example</span><span class="guides-sprite green-end-cap">&nbsp;</span></h3>
  <div class="block grey code" id="default-box-config-settings">

        web1: #component type & number
          name: pyrocms #component settings
          shared_writable_dirs:
            - /system/cms/cache/
            - /system/cms/logs/
            - /uploads/
          php_extensions:
            - mysql
            - gd
            - curl
        db1: #component type & number 
          name: articles #component settings
  </div>
	<p>Identifying shared_writable_dirs will allow your application to write cache, logs, images, uploads, etc. to those directories and then share them among application clones. The PHP extensions are those required to run PyroCMS</a>.</p>
	<p>Save the Boxfile, add new files to your repo, commit your changes, and push them to your PagodaBox repo.</p>
	<p>If you want to know more about writing your own or customizing your Boxfile, check out the <a href="/customer/portal/articles/175475">Boxfile Guide</a>.</p>
	<h4 id="-deploy-your-app-on-pagoda-box">Deploy Your App on Pagoda Box</h4>
	<p>Just go through the normal process of <a href="/customer/portal/articles/174146-launching-your-first-app">deploying your app</a>.</p>
	<h4 id="create-database">Create a Database</h4>
	<p>If you used the example Boxfile from above, an empty database has already been created for you. If not, you can manually <a href="/customer/portal/articles/175426-creating-a-database#creating-db-through-dashboard">create a database from the dashboard</a>.</p>
  <h4 id="connect-database">Connect to the Database</h4>
  <p>Since PyroCMS accommodates multiple sets of database credentials depending on the environment you're running in, you can use $db['live'] for your Pagoda Box credentials and $db['local'] for your local credentials. To make it even easier, you can use pre-created <a href="/customer/portal/articles/175470">global variables</a> instead of entering the credentials. But, you can also hard code the <a href="/customer/portal/articles/175426-creating-a-database#-connecting-to-your-db">credentials</a> if you prefer.</p>
  <p>Open the database configuration file located at /system/pyrocms/config/database.php and make the following changes.</p>
  <h3 id="database-settings" class="tag"><span class="guides-sprite cap">&nbsp;</span><span class="horizontal-guides-sprite title">PHP</span><span class="horizontal-guides-sprite green">Database Settings</span><span class="guides-sprite green-end-cap"></span></h3>
  <div class="block grey code">

            $db['live']['hostname'] = $_SERVER['DB1_HOST'];
            $db['live']['username'] = $_SERVER['DB1_USER'];
            $db['live']['password'] = $_SERVER['DB1_PASS'];
            $db['live']['database'] = $_SERVER['DB1_NAME'];
            $db['live']['port'] 	= $_SERVER['DB1_PORT'];
            ...
  </div>
	<h4 id="get-your-local-database-to-pagoda">Import your local database into Pagoda</h4>
	<p>Export your local database with your favorite local database administration tool(e.g. phpMyAdmin, Sequel Pro, etc.).</p>
	<p>Import your database into your live app on Pagoda using the <a href="/customer/portal/articles/175427">database tunnel</a></p>
	<p>Now when you go to yourapp.pagodabox.com you should see your PyroCMS app running on Pagoda!</p>
	<p>There&rsquo;s a few more things you can do to make your app fully functional that are covered in these other guides:<br />
			- <a href="/customer/portal/articles/175384">Sending Mail from Your App</a><br />
			- <a href="/customer/portal/articles/175471">Creating a DNS Alias</a><br />
			- <a href="/customer/portal/articles/175459">Scaling your App</a>(Coming)</p>
</div>

<h2 id="post-launch-workflow-recommendations">
	Post-launch Workflow Recommendations</h2>
<div class="justify">
	<h4 id="-things-to-note">Things to Note</h4>
	<p>Pagoda Box makes managing and updating your PyroCMS app really easy, but there&rsquo;s some things you should know.</p>
	<ol>
		<li>
			<p>For security reasons, Pagoda Box only allows SSH or SFTP access to your <a href="/customer/portal/articles/175418">writable directories</a>, but not your application code. This means that any updates, plugins, themes, or any other code changes need to be done on your local instance first, then pushed to your repo and deployed through your app dashboard.</p>
		</li>
		<li>
			<p>Remember that your local database and your live database are two completely separate databases. Anytime you want to publish a new post or add media to your PyroCMS app, it needs to be done through your live admin, not your local admin. Your PyroCMS content is not stored in your repo so any content you add locally will not be pushed live with your repo.</p>
		</li>
	</ol>
</div>