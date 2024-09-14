# Criando o Servidor de Webhook 🚀
Vamos criar um servidor Go que ouve as requisições HTTP no caminho /deploy e executa o script de deploy.

Código do Servidor de Webhook em Go:

~~~~GO
package main

import (
    "fmt"
    "log"
    "net/http"
    "os/exec"
)

func deploy() {
    // Aqui é onde o comando de deploy é executado
    cmd := exec.Command("/bin/sh", "deploy.sh")
    err := cmd.Run()
    if err != nil {
        log.Println("Erro ao rodar o deploy:", err)
    }
}

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Deploy iniciado!\n")
    go deploy() // Executa o deploy em uma goroutine
    log.Println("Deploy acionado por Webhook!")
}

func main() {
    http.HandleFunc("/deploy", handler) // Escuta na rota /deploy
    log.Println("Servidor de Webhook rodando na porta 5000")
    log.Fatal(http.ListenAndServe(":5000", nil)) // Escuta na porta 5000
}

~~~~
