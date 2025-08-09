# rifaoboticario
<?php
// Configuração da chave PIX
$chavePix = "89994131910";

// Carrega os números vendidos
$vendidosFile = "numeros_vendidos.txt";
$vendidos = [];
if (file_exists($vendidosFile)) {
    $vendidos = array_map('trim', file($vendidosFile));
}
?>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rifa Online</title>
<style>
    body { font-family: Arial, sans-serif; background: #f4f4f4; text-align: center; margin: 0; padding: 0; }
    header { background: #4CAF50; color: white; padding: 15px; font-size: 1.5em; }
    .grid { display: grid; grid-template-columns: repeat(10, 1fr); gap: 5px; max-width: 500px; margin: 20px auto; }
    .number { padding: 15px; background: white; border: 1px solid #ccc; cursor: pointer; font-weight: bold; transition: background 0.3s; }
    .number:hover { background: #d1ffd1; }
    .selected { background: #4CAF50; color: white; }
    .sold { background: #ff4d4d; color: white; cursor: not-allowed; }
    .pix-box { margin-top: 20px; background: white; padding: 15px; display: inline-block; border-radius: 10px; }
    input, button { padding: 10px; margin-top: 10px; width: 90%; }
</style>
</head>
<body>

<header>Rifa Online - Escolha seu número</header>

<div class="grid" id="numberGrid"></div>

<div class="pix-box">
    <h3>Pagamento via PIX</h3>
    <p>Chave PIX: <strong><?php echo $chavePix; ?></strong></p>
    <small>Copie a chave, faça o pagamento e envie o comprovante abaixo:</small>
    <form action="upload.php" method="POST" enctype="multipart/form-data">
        <input type="text" name="nome" placeholder="Seu Nome" required>
        <input type="text" name="numero" id="numeroEscolhido" placeholder="Número Escolhido" readonly required>
        <input type="file" name="comprovante" accept="image/*,application/pdf" required>
        <button type="submit">Enviar Comprovante</button>
    </form>
</div>

<script>
    const grid = document.getElementById('numberGrid');
    const numeroEscolhido = document.getElementById('numeroEscolhido');
    const vendidos = <?php echo json_encode($vendidos); ?>;

    for (let i = 1; i <= 100; i++) {
        let btn = document.createElement('div');
        btn.classList.add('number');
        btn.textContent = i;

        if (vendidos.includes(String(i))) {
            btn.classList.add('sold');
            btn.textContent = i + " ❌";
        } else {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.number').forEach(n => n.classList.remove('selected'));
                btn.classList.add('selected');
                numeroEscolhido.value = i;
            });
        }
        grid.appendChild(btn);
    }
</script>

</body>
</html>
