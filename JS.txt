
document.getElementById('limparAluno').addEventListener('click', () => {
  document.getElementById('alunoForm').reset();
  document.getElementById('endAluno').value = '';
});



document.getElementById('alunoForm').addEventListener('submit', async (e) => {
  e.preventDefault();
  const payload = {
    nome: document.getElementById('nomeAluno').value,
    email: document.getElementById('emailAluno').value,
    telefone: document.getElementById('telefoneAluno').value,
    plano: document.getElementById('planoAluno').value,
    endereco: document.getElementById('endAluno').value
  };

  try {
    const url = (API_BASE || '') + '/api/alunos';
    if (!API_BASE) {
      alert('Demo: payload enviado (API_BASE não configurada):\\n' + JSON.stringify(payload, null, 2));
      return;
    }
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });
    const json = await res.json();
    if (res.ok) {
      alert('Aluno cadastrado com sucesso!');
      document.getElementById('alunoForm').reset();
      document.getElementById('endAluno').value = '';
    } else {
      alert('Erro ao cadastrar: ' + JSON.stringify(json));
    }
  } catch (err) {
    console.error(err);
    alert('Falha no cadastro. Veja o console.');
  }
});



// Consulta CEP (ViaCEP)
document.getElementById('btnCepAluno').addEventListener('click', async () => {
  const cep = document.getElementById('cepAluno').value.replace(/\\D/g, '');
  if (cep.length !== 8) {
    alert('Informe um CEP válido com 8 dígitos.');
    return;
  }
  try {
    const res = await fetch(`https://viacep.com.br/ws/${cep}/json/`);
    const json = await res.json();
    if (json.erro) {
      document.getElementById('endAluno').value = 'CEP não encontrado.';
      return;
    }
    const formatted = `${json.logradouro || ''} ${json.bairro || ''} ${json.localidade || ''}-${json.uf || ''}`.trim();
    document.getElementById('endAluno').value = formatted;
  } catch (err) {
    console.error(err);
    alert('Erro ao consultar o CEP.');
  }
});
