<?php

$pagina = 1;

if (isset($_GET['pagina']))
$pagina = filter_input(INPUT_GET, "pagina", FILTER_VALIDATE_INT);
if(!$pagina)
$pagina = 1;

$limite = 4;

$inicio = ($pagina * $limite) - $limite;

$conn = new mysqli("", "", "", "");

$registros = $conn->query("select COUNT(user_login) as count from wp__users");
$total_registros = mysqli_fetch_array($registros)["count"];

$paginas = ceil($total_registros / $limite);

$sql = "select user_login, user_email from wp__users order by user_login LIMIT $inicio, $limite";
$result = $conn->query($sql);
  
?>

<div id="records">

<table class="table table-hover">
  <thead>
    <tr>
      <th scope="col">User Login</th>
      <th scope="col">User Email</th>
    </tr>
  </thead>
  <tbody>

    <?php while($item = mysqli_fetch_array($result)){ ?>    
    <tr>
      <td><?=$item["user_login"]?></td>
      <td><?=$item["user_email"]?></td>
    </tr>
    <?php } ?>

  </tbody>

</table>

<a href="?pagina=1">Primeira</a>
<a href="?pagina=<?=$pagina -1?>"><<</a>

<?=$pagina ?>

<a href="?pagina=<?=$pagina +1?>">>></a>
<a href="?pagina=<?=$paginas ?>">Ultima</a>

</div>
