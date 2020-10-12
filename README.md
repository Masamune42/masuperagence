# Cours Symfony Grafikart

## Extensions utiles
- Twig Langugage : coloration des fichiers twig

## Soucis rencontrés + corrections
- 3. Découverte de doctrine : 
    - Erreur : Conflit lors de l'ajout du package slugify
    - Solution : Installer une version antérieurs : composer require cocur/slugify:3.1

## Tips
### Symfony :
- Ajouter un style Bootstrap à une formulaire
    - Dans : config/packages/twig.yaml
    ```yaml
    form_theme: ['bootstrap_4_layout.html.twig']
    ```
    - Dans le fichier html.twig :
    ````twig
    <div class="container mt-4">
        <h1>Editer le bien</h1>
        {{ form_start(form) }}
        {{ form_widget(form) }}
        <button class="btn btn-primary">Editer</button>
        {{ form_end(form) }}
    </div>
    ````

- Ajouter des traductions (ex : form) pour changer les champs des formulaires (solution 1)
    - Créer un fichier : forms.fr.yaml avec les correspondances -> ex : City: Ville
    - Dans config\services.yaml : 
    ```yaml
    parameters:
        locale: 'fr'
    ```
    - Dans config\packages\translation.yaml :
    ```yaml
    framework:
    default_locale: '%locale%'
    translator:
        default_path: '%kernel.project_dir%/translations'
        fallbacks:
            - '%locale%'
    ```
    - Dans src\Form\PropertyType.php : 
    ```php
    $resolver->setDefaults([
            'data_class' => Property::class,
            'translation_domain' => 'forms'
    ]);
    ```

- Pour changer les champs des formulaires (solution 2)
    - Dans src\Form\PropertyType.php :
    ```php
        // 1. paramètre, 2. type, 3. label
        $builder
        ->add('city', null, [
            'label' => 'Ville'
        ])
        // On défini un type ChoiceType -> liste déroulante
        ->add('heat', ChoiceType::class, [
                'choices' => array_flip(Property::HEAT)
        ])
    ```

### PHP :
- array_flip : Remplace les clés par les valeurs, et les valeurs par les clés