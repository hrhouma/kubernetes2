# Sticky-bit.md ?
- Le sticky bit est un paramètre de permission sur un fichier ou un dossier dans les systèmes de fichiers Unix et Linux.
- Lorsqu'il est défini sur un dossier, il empêche les utilisateurs de supprimer ou de renommer des fichiers dans ce dossier, à moins qu'ils ne soient le propriétaire du fichier, le propriétaire du dossier, ou un super-utilisateur.
- Cette fonctionnalité est particulièrement utile dans des dossiers comme `/tmp`, où tous les utilisateurs ont besoin d'écrire des données, mais ne devraient pas être en mesure de supprimer ou de modifier les fichiers des autres.
- Pour un fichier exécutable, lorsqu'on lui attribue le sticky bit, une copie du programme exécuté reste en mémoire.
- Cela était utilisé dans les systèmes plus anciens pour accélérer le redémarrage des programmes fréquemment utilisés, bien que cet usage soit moins courant sur les systèmes modernes en raison de l'évolution de la gestion de la mémoire.

# Application
Pour voir si un fichier ou un dossier a le sticky bit défini, vous pouvez utiliser la commande `ls -l`. Un `t` dans les permissions (par exemple, `drwxrwxrwt`) indique que le sticky bit est défini. Pour définir le sticky bit sur un dossier, vous pouvez utiliser la commande `chmod +t <dossier>`.
