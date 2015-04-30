# backOffice page d'index 
<html>
	<head>
    	<meta http-equiv="content-type" content="text/html; charset=UTF-8">
    	<link type="text/css" rel="stylesheet" href="CSS/style.css" />
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
   		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
    	<title>GEEKADVISOR</title>
  	</head>
	
 	<body>
 		<nav class="navbar navbar-default" role="navigation">
  			<div class="container-fluid">
				<div class="navbar-header">
      					<button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
       					<span class="sr-only">Toggle navigation</span>
        				<span class="icon-bar"></span>
        				<span class="icon-bar"></span>
        				<span class="icon-bar"></span>
      					</button>
    				</div>

    				<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      					<form id="form_co" class="navbar-form navbar-right" role="form" method="POST" action="index.php">
							<div id="Champs_co">
            					<div class="form-group">
              						<input type="text" placeholder="Pseudo" name ="user_co" class="form-control" required pattern="[a-zA-Z0-9]{3,20}">
            					</div>
       							<div class="form-group">
       								<input type="password" placeholder="Password" name ="pass_co" class="form-control" required pattern="[a-zA-Z0-9]{3,20}">
       							</div>
            					<button type="submit" name="connection" class="btn btn-success">Se connecter</button>	
							</div>
							<p></p>
          				</form>
						<form class="navbar-form navbar-right" id="inscrire" action = "register.php" method = "POST">
							<button type="submit"  class="btn btn-info">S'inscrire</button>
						</form>
    				</div><!-- /.navbar-collapse -->
  			</div><!-- /.container-fluid -->
		</nav>
		
		<div id = "contenue">
			<div id="alerte_message" class="alert alert-danger fade in">
      				<h4>Une erreur dans votre formulaire est survenue!</h4>
      				<p></p>
    		</div>
    		<div class="jumbotron">
				<h1>Bienvenue sur <strong>GeekAdisor</strong></h1>
			</div>
			</br>	
		</div>
	<?php
		session_start();

		if(isset($_SESSION['user_co']))
		{
			echo '<script type="text/javascript">
			$("#alerte_message").hide();
			$("#Champs_co").hide();
			$("#inscrire").hide();
			var id = document.getElementById("form_co");
			var p = id.getElementsByTagName("p")[0];						
			p.innerHTML="Bienvenue <u>';
			echo	$_SESSION['user_co'];
			echo    '</u><a href=\"logout.php\"> Se deconnecter</a>";</script>';
		}

		if(isset($_POST['connection']))
		{
			$username = htmlspecialchars(trim($_POST['user_co']));
			$pass = htmlspecialchars(trim($_POST['pass_co']));
			
			if (empty($username) || empty($pass))
			{
				echo '<script type="text/javascript">
				var id = document.getElementById("alerte_message");
				var p = id.getElementsByTagName("p")[0];
				p.innerHTML="Pseudo et/ou Password manquant";
				$("#alerte_message").fadeIn("slow");
				</script>';
			}
			else
			{
				require 'base.php';
				$pass= sha1($pass);
				try
				{
					// connexion PDO
					$cnx = new PDO (DSN,LOGIN,PASSWORD,$options);
					// preparation requete
					$req = "SELECT * FROM utilisateur WHERE login =:login AND password =:pass";
					$req_prep = $cnx->prepare($req);
					// lier les paramètres
					$req_prep->bindParam(':login',$username,PDO::PARAM_STR);
					$req_prep->bindParam(':pass',$pass,PDO::PARAM_STR);
					// execution de la requete
					$req_prep->execute();
					// recuperation des resultats
					$res = $req_prep->fetch();

					if($res === false )
					{
						echo '<script type="text/javascript">
						var id = document.getElementById("alerte_message");
						var p = id.getElementsByTagName("p")[0];
						p.innerHTML="Erreur authentification";
						$("#alerte_message").fadeIn("slow");
						</script>';
					}
					else
					{
						$_SESSION ['user_co']= $username;
					
						echo '<script type="text/javascript">
						$("#alerte_message").hide();
						$("#Champs_co").hide();
						var id = document.getElementById("form_co");
						var p = id.getElementsByTagName("p")[0];						
						p.innerHTML="Bienvenue <u>';
						echo	$_SESSION['user_co'];
						echo	' ';
						echo	' ';
						echo	' ';
						echo	' ';
						echo	' ';
						echo	' ';
						echo	' ';
						echo	' ';
						echo    '</u><a href=\"logout.php\"> Se deconnecter</a>";</script>';

						echo    '<center><a href="ajouter_support.php"> Cliquez ici </a></center>;</script>';
					}	

				}
				catch ( PDOException $e)	
				{
					die('echec :'.$e->getMessage());
				}			
			}
		}
	?>
   		<!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
   		<script src="https://code.jquery.com/jquery.js"></script>
  		 <!-- Include all compiled plugins (below), or include individual files as needed -->
  		 <script src="//netdna.bootstrapcdn.com/bootstrap/3.1.1/js/bootstrap.min.js"></script>
 	</body>

</html>
