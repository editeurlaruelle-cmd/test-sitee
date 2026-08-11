const { exec } = require("child_process");

const REPO = "https://github.com/editeurlaruelle-cmd/test-sitee.git";
const SITE_DIR = "/home/kylian/Desktop/test";

let lastCommit = null;

function command(cmd) {
    return new Promise((resolve, reject) => {
        exec(cmd, (error, stdout, stderr) => {
            if (error) {
                reject(error);
                return;
            }
            resolve(stdout.trim());
        });
    });
}

async function checkUpdate() {
    try {
        // Récupère les informations du dépôt
        await command(`git -C "${SITE_DIR}" fetch origin`);

        // Récupère le dernier commit GitHub
        const remoteCommit = await command(
            `git -C "${SITE_DIR}" rev-parse origin/main`
        );

        if (lastCommit === null) {
            lastCommit = remoteCommit;
            console.log("Version actuelle :", lastCommit);
            return;
        }

        if (remoteCommit !== lastCommit) {
            console.log("🚀 Mise à jour détectée !");
            console.log("Ancien :", lastCommit);
            console.log("Nouveau :", remoteCommit);

            // Met à jour le site
            await command(`git -C "${SITE_DIR}" pull origin main`);

            lastCommit = remoteCommit;

            console.log("✅ Site mis à jour !");
        } else {
            console.log("✔️ Aucune mise à jour.");
        }

    } catch (error) {
        console.error("❌ Erreur :", error.message);
    }
}

// Vérification toutes les 30 secondes
checkUpdate();
setInterval(checkUpdate, 30_000);