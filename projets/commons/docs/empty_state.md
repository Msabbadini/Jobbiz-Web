# Composant EmptyState

Le composant `EmptyState` est utilisé pour afficher des états vides avec des icônes et des messages spécifiques en fonction du type d'état.

## Utilisation

Ce composant est particulièrement utile pour informer les utilisateurs lorsqu'il n'y a pas de données à afficher, comme les missions terminées, en cours, ou les contrats signés.

## Props

- **type** (string) : Définit le type d'état vide à afficher. Les valeurs possibles incluent `mission_done`, `mission_in_progress`, `mission_soon`, `mission_search`, `contract`, `time_sheet`, `payslip`, `coming_soon`, `select_mission`, et `log_vivier`.
- **children** (node) : Permet d'insérer des éléments enfants supplémentaires si nécessaire.
- **...props** : Toute autre prop peut être passée au composant.


```javascript
import React from "react";
import Button from "components/button";
import {
    EmptyStateComingSoon,
    EmptyStateContract,
    EmptyStateMission,
    EmptyStateMissionSearch,
    EmptyStatePayslip,
    EmptyStateTimeRecord
} from "assets/icons";
import { useNavigate } from "react-router-dom";

// Composant EmptyState pour afficher des états vides avec des icônes et des messages spécifiques
const EmptyState = ({ type, children, ...props }) => {
    const navigate = useNavigate();

    // Objet contenant les icônes à afficher en fonction du type
    const logo = {
        'mission_done': <div className={'mx-auto my-2'}><EmptyStateMission wh={256} /></div>,
        'mission_in_progress': <div className={'mx-auto my-2'}><EmptyStateMission wh={256} /></div>,
        'mission_soon': <div className={'mx-auto my-2'}><EmptyStateMission wh={256} /></div>,
        'mission_search': <div className={'mx-auto my-2'}><EmptyStateMissionSearch wh={256} /></div>,
        'contract': <div className={'mx-auto my-2'}><EmptyStateContract wh={256} /></div>,
        'time_sheet': <div className={'mx-auto my-2'}><EmptyStateTimeRecord wh={256} /></div>,
        'payslip': <div className={'mx-auto my-2'}><EmptyStatePayslip wh={256} /></div>,
        'coming_soon': <div className={'mx-auto my-2'}><EmptyStateComingSoon wh={256} /></div>,
        'select_mission': <div className={'mx-auto my-2'}><EmptyStateMissionSearch wh={256} /></div>,
    };

    // Objet contenant les titres à afficher en fonction du type
    const title = {
        'mission_done': 'Aucune mission terminées',
        'mission_in_progress': 'Aucune mission en cours',
        'mission_soon': 'Aucune mission prochainement',
        'mission_search': 'Pas de missions disponibles',
        'contract': 'Aucun contrat signé disponible',
        'time_sheet': 'Aucun relevé d’heure à compléter',
        'payslip': 'Aucun fiche de paie',
        'coming_soon': 'Fonctionnalité bientôt disponible',
        'select_mission': 'Sélectionnez une mission',
        'log_vivier': 'Aucune notification de vivier',
    };

    // Objet contenant les descriptions à afficher en fonction du type
    const description = {
        'mission_done': 'Oops, vous n’avez pas encore de missions terminées, pour en avoir il faut postuler à une mission !',
        'mission_in_progress': 'Oops, vous n’avez pas encore de missions en cours, pour en avoir il faut postuler à une mission !',
        'mission_soon': 'Oops, vous n\'avez pas encore de missions prochainement, pour en avoir, il faut postuler à une mission !',
        'mission_search': 'Oops, il n’y a pas encore de missions disponibles pour le moment, un peu de patience',
        'contract': 'Oops, il n’y a pas encore de missions disponibles pour le moment, un peu de patience',
        'time_sheet': 'Oops, il n’y a pas encore de missions disponibles pour le moment, un peu de patience',
        'payslip': 'Oops, il n’y a pas encore de missions disponibles pour le moment, un peu de patience',
        'coming_soon': 'Vous serez notifié une fois que cette fonctionnalité sera disponible.',
        'select_mission': 'Sélectionnez une mission pour exécuter une action.',
        'log_vivier': 'Oops, il n’y a pas encore de notification de viviers pour le moment.',
    };

    // Objet contenant les boutons à afficher en fonction du type
    const button = {
        'mission_done': <div className={'my-3'}><Button color={'PRIMARY'} size={'LG'} onClick={e => navigate('/searchMission')}>Rechercher de missions</Button></div>,
        'mission_in_progress': <div className={'my-3'}><Button color={'PRIMARY'} size={'LG'} onClick={e => navigate('/searchMission')}>Rechercher de missions</Button></div>,
        'mission_soon': <div className={'my-3'}><Button color={'PRIMARY'} size={'LG'} onClick={e => navigate('/searchMission')}>Rechercher de missions</Button></div>,
        'contract': '',
        'time_sheet': '',
        'payslip': '',
        'coming_soon': '',
    };

    return (
        <>
            <div className={'max-w-[535px] my-16 mx-auto text-center px-3'}>
                <div className={'flex'}>
                    {logo[type]}
                </div>
                <div className={'font-medium ft-xl text-[#5C616D]'}>
                    {title[type]}
                </div>
                <div className={'font-normal ft-b mt-2 text-[#757575]'}>
                    {description[type]}
                </div>
                <div className={'flex'}>
                    <div className={'mx-auto'}>
                        {button[type]}
                    </div>
                </div>
            </div>
        </>
    );
};

export default EmptyState;

```


## Exemple d'Utilisation

```javascript
<EmptyState type="mission_done" />
```