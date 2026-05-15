# Codigos-para-bueiros-inteligentes
Código em C++ para funcionamento do circuito do sensor + pseudocódigo para funcionamento do sensor junto com a API de clima  
// Definindo os pinos do Sensor Ultrassônico
const int pinoTrig = 7;
const int pinoEcho = 6;

// Definindo os pinos dos LEDs
const int ledVerde = 12;
const int ledAmarelo = 11;
const int ledVermelho = 10;

// ==========================================
// CONFIGURAÇÃO DE TEMPO (Simulação de Sleep)
// ==========================================
// Para testar rápido no Tinkercad, deixei em 5000 (5 segundos)
// Para o projeto real de 15 minutos, basta alterar para 900000 
long tempoDeDescanso = 5000; 

void setup() {
  Serial.begin(9600);
  
  pinMode(pinoTrig, OUTPUT);
  pinMode(pinoEcho, INPUT);
  
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
}

void loop() {
  long duracao;
  int distancia;

  Serial.println("Acordando e fazendo leitura...");

  // Emite o pulso sonoro do sensor
  digitalWrite(pinoTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinoTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinoTrig, LOW);

  // Calcula o eco e a distância
  duracao = pulseIn(pinoEcho, HIGH);
  distancia = duracao * 0.034 / 2;

  Serial.print("Distancia do Lixo: ");
  Serial.print(distancia);
  Serial.println(" cm");

  // LÓGICA DO BUEIRO INTELIGENTE
  if (distancia > 80) {
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
  } 
  else if (distancia >= 30 && distancia <= 80) {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
  } 
  else {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
  }

  // Mantém os LEDs acesos por 2 segundos para dar tempo de visualizar o alerta
  delay(2000); 

  // ==========================================
  // ENTRANDO EM MODO DESCANSO (DEEP SLEEP)
  // ==========================================
  Serial.println("Entrando em Deep Sleep para poupar bateria...");
  
  // 1. Desliga todos os LEDs para não gastar energia
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, LOW);

  // 2. O Arduino "dorme" pelo tempo configurado
  delay(tempoDeDescanso); 
}

Esse código faz com que o circuito funcione para reconhecer quando o bueiro está ficando com sua capacidade máxima de lixo  
Nesse trabalho, buscamos uma forma de acabar com os alagamentos nas cidades.
