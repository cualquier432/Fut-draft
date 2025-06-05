import React, { useState, useEffect, useCallback } from 'react';

// Función para generar un color aleatorio para el placeholder de la imagen
const getRandomHexColor = () => {
    const letters = '0123456789ABCDEF';
    let color = '';
    for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
};

// Datos de jugadores con media, posición y URL de imagen placeholder
const ALL_PLAYERS = [
    // Porteros (GK)
    { id: 'p1', name: 'Alisson Becker', position: 'GK', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ALI` },
    { id: 'p2', name: 'Thibaut Courtois', position: 'GK', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=COU` },
    { id: 'p3', name: 'Marc-André ter Stegen', position: 'GK', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=TER` },
    { id: 'p4', name: 'Jan Oblak', position: 'GK', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=OBL` },
    { id: 'p5', name: 'Ederson', position: 'GK', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=EDE` },
    { id: 'p48', name: 'Mike Maignan', position: 'GK', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAI` },
    { id: 'p49', name: 'Keylor Navas', position: 'GK', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NAV` },
    { id: 'p50', name: 'David Soria', position: 'GK', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SOR` },
    { id: 'p51', name: 'Neto', position: 'GK', rating: 75, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NET` },
    { id: 'p52', name: 'Iñaki Peña', position: 'GK', rating: 70, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=IPE` },
    { id: 'p53', name: 'Paulo Gazzaniga', position: 'GK', rating: 65, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAZ` },
    { id: 'p54', name: 'Fernando Pacheco', position: 'GK', rating: 60, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PAC` },
    { id: 'p55', name: 'Jon Ander Felipe', position: 'GK', rating: 55, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=JAF` },
    { id: 'p56', name: 'Leo Román', position: 'GK', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ROM` },
    { id: 'p181', name: 'Arnau Tenas', position: 'GK', rating: 72, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ATE` },
    { id: 'p182', name: 'Álex Remiro', position: 'GK', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ARE` },

    // Defensas Centrales (CB)
    { id: 'p6', name: 'Virgil van Dijk', position: 'CB', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VVD` },
    { id: 'p7', name: 'Ruben Dias', position: 'CB', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DIA` },
    { id: 'p8', name: 'Marquinhos', position: 'CB', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAR` },
    { id: 'p9', name: 'Eder Militão', position: 'CB', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MIL` },
    { id: 'p10', name: 'Ronald Araujo', position: 'CB', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ARA` },
    { id: 'p11', name: 'Matthijs de Ligt', position: 'CB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DLG` },
    { id: 'p12', name: 'Jules Kounde', position: 'CB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KOU` },
    { id: 'p57', name: 'Antonio Rüdiger', position: 'CB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RÜD` },
    { id: 'p58', name: 'Pau Torres', position: 'CB', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=TOR` },
    { id: 'p59', name: 'David Alaba', position: 'CB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ALA` },
    { id: 'p60', name: 'Aymeric Laporte', position: 'CB', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LAP` },
    { id: 'p61', name: 'Diego Carlos', position: 'CB', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAR` },
    { id: 'p62', name: 'Mario Hermoso', position: 'CB', rating: 79, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=HER` },
    { id: 'p63', name: 'Robin Le Normand', position: 'CB', rating: 77, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NOR` },
    { id: 'p64', name: 'Juanpe', position: 'CB', rating: 72, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=JUA` },
    { id: 'p65', name: 'Germán Pezzella', position: 'CB', rating: 68, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PEZ` },
    { id: 'p66', name: 'Unai Nuñez', position: 'CB', rating: 62, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NUN` },
    { id: 'p67', name: 'Víctor Chust', position: 'CB', rating: 55, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CHU` },
    { id: 'p103', name: 'Gonzalo Escalante', position: 'CB', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ESC` },
    { id: 'p125', name: 'Pau Cubarsí', position: 'CB', rating: 74, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CUB` },
    { id: 'p129', name: 'Joško Gvardiol', position: 'CB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GVA` },
    { id: 'p135', name: 'Levi Colwill', position: 'CB', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=COL` },
    { id: 'p141', name: 'Gabriel Magalhães', position: 'CB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAB` },
    { id: 'p144', name: 'Raphaël Varane', position: 'CB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VAR` },
    { id: 'p149', name: 'Victor Nelsson', position: 'CB', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NEL` },
    { id: 'p160', name: 'Piero Hincapié', position: 'CB', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=HIN` },
    { id: 'p161', name: 'Evan Ndicka', position: 'CB', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NDI` },
    { id: 'p168', name: 'Niklas Süle', position: 'CB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SÜL` },
    { id: 'p169', name: 'Dayot Upamecano', position: 'CB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=UPA` },
    { id: 'p174', name: 'Alessandro Bastoni', position: 'CB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BAS` },
    { id: 'p183', name: 'William Saliba', position: 'CB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SAL` },
    { id: 'p184', name: 'Cristian Romero', position: 'CB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ROM` },


    // Laterales Derechos (RB)
    { id: 'p13', name: 'Trent Alexander-Arnold', position: 'RB', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=TAA` },
    { id: 'p14', name: 'Achraf Hakimi', position: 'RB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=HAK` },
    { id: 'p15', name: 'Kyle Walker', position: 'RB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WAL` },
    { id: 'p16', name: 'João Cancelo', position: 'RB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAN` },
    { id: 'p68', name: 'Dani Carvajal', position: 'RB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAR` },
    { id: 'p69', name: 'Reece James', position: 'RB', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=JAM` },
    { id: 'p70', name: 'Jeremie Frimpong', position: 'RB', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FRI` },
    { id: 'p71', name: 'Pedro Porro', position: 'RB', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=POR` },
    { id: 'p72', name: 'Jesús Navas', position: 'RB', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NAV` },
    { id: 'p73', name: 'Nacho Vidal', position: 'RB', rating: 73, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VID` },
    { id: 'p74', name: 'Álvaro Odriozola', position: 'RB', rating: 68, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ODR` },
    { id: 'p75', name: 'Miguel Gutiérrez', position: 'RB', rating: 60, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GUT` },
    { id: 'p104', name: 'Rubén Sánchez', position: 'RB', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RUB` },
    { id: 'p137', name: 'Ben White', position: 'RB', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WHI` },
    { id: 'p138', name: 'Denzel Dumfries', position: 'RB', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DUM` },
    { id: 'p142', name: 'Diogo Dalot', position: 'RB', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DAL` },
    { id: 'p162', name: 'Noussair Mazraoui', position: 'RB', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAZ` },
    { id: 'p185', name: 'Sergi Roberto', position: 'RB', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SRO` },
    { id: 'p186', name: 'Arnau Martinez', position: 'RB', rating: 76, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=AMA` },

    // Laterales Izquierdos (LB)
    { id: 'p17', name: 'Alphonso Davies', position: 'LB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DAV` },
    { id: 'p18', name: 'Andrew Robertson', position: 'LB', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ROB` },
    { id: 'p19', name: 'Theo Hernández', position: 'LB', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=THE` },
    { id: 'p76', name: 'Ferland Mendy', position: 'LB', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MEN` },
    { id: 'p77', name: 'Alejandro Balde', position: 'LB', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BAL` },
    { id: 'p78', name: 'José Gayà', position: 'LB', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAY` },
    { id: 'p79', name: 'Fran García', position: 'LB', rating: 76, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FRA` },
    { id: 'p80', name: 'Alfonso Pedraza', position: 'LB', rating: 71, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PED` },
    { id: 'p81', name: 'Jaume Costa', position: 'LB', rating: 65, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=COS` },
    { id: 'p105', name: 'Arnau Solà', position: 'LB', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SOL` },
    { id: 'p136', name: 'Destiny Udogie', position: 'LB', rating: 77, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=UDO` },
    { id: 'p143', name: 'Luke Shaw', position: 'LB', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SHA` },
    { id: 'p187', name: 'Álex Moreno', position: 'LB', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=AMO` },
    { id: 'p188', name: 'Cucurella', position: 'LB', rating: 79, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CUC` },

    // Centrocampistas Defensivos (CDM) / Medios Centros (CM) / Medios Ofensivos (CAM)
    { id: 'p20', name: 'Rodri', position: 'CDM', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ROD` },
    { id: 'p21', name: 'Casemiro', position: 'CDM', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAS` },
    { id: 'p22', name: 'Joshua Kimmich', position: 'CDM', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KIM` },
    { id: 'p23', name: 'Frenkie de Jong', position: 'CM', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DEJ` },
    { id: 'p24', name: 'Federico Valverde', position: 'CM', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VAL` },
    { id: 'p25', name: 'Bruno Fernandes', position: 'CAM', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BF` },
    { id: 'p26', name: 'Toni Kroos', position: 'CM', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KRO` },
    { id: 'p27', name: 'Luka Modrić', position: 'CM', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MOD` },
    { id: 'p28', name: 'Kevin De Bruyne', position: 'CAM', rating: 91, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KDB` },
    { id: 'p29', name: 'Bernardo Silva', position: 'CAM', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SIL` },
    { id: 'p30', name: 'Jamal Musiala', position: 'CAM', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MUS` },
    { id: 'p45', name: 'Jude Bellingham', position: 'CM', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BEL` },
    { id: 'p46', name: 'Florian Wirtz', position: 'CAM', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WIR` },
    { id: 'p82', name: 'Ilkay Gündoğan', position: 'CM', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GUN` },
    { id: 'p83', name: 'Pedri', position: 'CM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PED` },
    { id: 'p84', name: 'Gavi', position: 'CM', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAV` },
    { id: 'p85', name: 'Gabri Veiga', position: 'CM', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VEI` },
    { id: 'p86', name: 'Oihan Sancet', position: 'CAM', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SAN` },
    { id: 'p87', name: 'Samu Costa', position: 'CDM', rating: 75, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=COS` },
    { id: 'p88', name: 'Unai Vencedor', position: 'CDM', rating: 70, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VEN` },
    { id: 'p89', name: 'Manu Morlanes', position: 'CM', rating: 65, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MOR` },
    { id: 'p90', name: 'Iván Morante', position: 'CM', rating: 60, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=IMO` },
    { id: 'p91', name: 'Alberto Soro', position: 'CAM', rating: 55, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SOR` },
    { id: 'p92', name: 'Óscar Aranda', position: 'CAM', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ARA` },
    { id: 'p123', name: 'Gavi', position: 'CM', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAV` },
    { id: 'p127', name: 'Warren Zaïre-Emery', position: 'CM', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WZE` },
    { id: 'p128', name: 'Arda Güler', position: 'CAM', rating: 76, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GUL` },
    { id: 'p130', name: 'Fede Valverde', position: 'CM', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VAL` }, // Duplicate for more options in CM
    { id: 'p131', name: 'Cole Palmer', position: 'CAM', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PAL` },
    { id: 'p132', name: 'Alexis Mac Allister', position: 'CM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAC` },
    { id: 'p133', name: 'Julian Brandt', position: 'CAM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BRA` },
    { id: 'p134', name: 'Declan Rice', position: 'CDM', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RIC` },
    { id: 'p139', name: 'Pierre-Emile Højbjerg', position: 'CDM', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=HOJ` },
    { id: 'p140', name: 'Fabinho', position: 'CDM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FAB` },
    { id: 'p150', name: 'Emre Can', position: 'CDM', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAN` },
    { id: 'p151', name: 'Youssouf Fofana', position: 'CM', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FOF` },
    { id: 'p153', name: 'Dominik Szoboszlai', position: 'CAM', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SZO` },
    { id: 'p155', name: 'Enzo Fernández', position: 'CM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ENZ` },
    { id: 'p157', name: 'Sandro Tonali', position: 'CDM', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=TON` },
    { id: 'p158', name: 'Nicolo Barella', position: 'CM', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BAR` },
    { id: 'p176', name: 'Javier Pastore', position: 'CAM', rating: 72, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PAS` }, // Older player
    { id: 'p177', name: 'Shinji Kagawa', position: 'CAM', rating: 68, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KAG` }, // Older player
    { id: 'p178', name: 'Jack Wilshere', position: 'CM', rating: 60, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WIL` }, // Older player
    { id: 'p180', name: 'Juan Mata', position: 'CAM', rating: 70, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAT` }, // Older player
    { id: 'p189', name: 'Aurélien Tchouaméni', position: 'CDM', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=TCH` },
    { id: 'p190', name: 'Eduardo Camavinga', position: 'CM', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAM` },
    { id: 'p191', name: 'Gavi', position: 'CM', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAV` }, // Duplicate for more options
    { id: 'p192', name: 'Jude Bellingham', position: 'CAM', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BEL` }, // Duplicate for more options
    { id: 'p193', name: 'Moises Caicedo', position: 'CDM', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAI` },


    // Extremos Derechos (RW)
    { id: 'p31', name: 'Lionel Messi', position: 'RW', rating: 94, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MES` }, // Rating 94 for iconic player
    { id: 'p32', name: 'Mohamed Salah', position: 'RW', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SAL` },
    { id: 'p33', name: 'Bukayo Saka', position: 'RW', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SAK` },
    { id: 'p93', name: 'Raphinha', position: 'RW', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RAP` },
    { id: 'p94', name: 'Marcos Llorente', position: 'RW', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LLO` }, // Versatile
    { id: 'p95', name: 'Ferran Torres', position: 'RW', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FER` },
    { id: 'p96', name: 'Iñaki Williams', position: 'RW', rating: 79, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WIL` },
    { id: 'p97', name: 'Bryan Gil', position: 'RW', rating: 76, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GIL` },
    { id: 'p98', name: 'Nico Williams', position: 'RW', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NWI` },
    { id: 'p106', name: 'Takefusa Kubo', position: 'RW', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KUB` },
    { id: 'p107', name: 'Arnau Puigmal', position: 'RW', rating: 65, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=APU` },
    { id: 'p108', name: 'Jaime Seoane', position: 'RW', rating: 55, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SEO` },
    { id: 'p109', name: 'Roberto López', position: 'RW', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RLO` },
    { id: 'p124', name: 'Lamine Yamal', position: 'RW', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LYA` },
    { id: 'p145', name: 'Riyad Mahrez', position: 'RW', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAH` },
    { id: 'p164', name: 'Ousmane Dembélé', position: 'RW', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DEM` },
    { id: 'p167', name: 'Serge Gnabry', position: 'RW', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GNA` },
    { id: 'p173', name: 'Federico Chiesa', position: 'RW', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CHI` },
    { id: 'p175', name: 'Theo Walcott', position: 'RW', rating: 70, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=WAL` },


    // Extremos Izquierdos (LW)
    { id: 'p34', name: 'Vinicius Jr.', position: 'LW', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VIN` },
    { id: 'p35', name: 'Neymar Jr.', position: 'LW', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NEY` },
    { id: 'p36', name: 'Rafael Leão', position: 'LW', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LEA` },
    { id: 'p99', name: 'Khvicha Kvaratskhelia', position: 'LW', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KVA` },
    { id: 'p100', name: 'Ansu Fati', position: 'LW', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FAT` },
    { id: 'p101', name: 'Yannick Carrasco', position: 'LW', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAR` },
    { id: 'p102', name: 'Rodrygo', position: 'LW', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ROD` },
    { id: 'p110', name: 'Mikel Oyarzabal', position: 'LW', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=OYA` },
    { id: 'p111', name: 'Samuel Lino', position: 'LW', rating: 79, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LIN` },
    { id: 'p112', name: 'Manu Sánchez', position: 'LW', rating: 60, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MSA` },
    { id: 'p113', name: 'Álvaro Fernández', position: 'LW', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=AFE` },
    { id: 'p126', name: 'Alejandro Garnacho', position: 'LW', rating: 79, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GAR` },
    { id: 'p146', name: 'Jack Grealish', position: 'LW', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GRE` },
    { id: 'p147', name: 'Phil Foden', position: 'LW', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FOD` },
    { id: 'p163', name: 'Ivan Perišić', position: 'LW', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PER` },
    { id: 'p165', name: 'Kingsley Coman', position: 'LW', rating: 86, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=COM` },
    { id: 'p166', name: 'Leroy Sané', position: 'LW', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=SAN` },
    { id: 'p171', name: 'Christian Pulisic', position: 'LW', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=PUL` },


    // Delanteros Centrales (ST)
    { id: 'p37', name: 'Kylian Mbappé', position: 'ST', rating: 91, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MBP` },
    { id: 'p38', name: 'Erling Haaland', position: 'ST', rating: 91, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=HAA` },
    { id: 'p39', name: 'Robert Lewandowski', position: 'ST', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LEW` },
    { id: 'p40', name: 'Karim Benzema', position: 'ST', rating: 89, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BEN` },
    { id: 'p42', name: 'Harry Kane', position: 'ST', rating: 90, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KAN` },
    { id: 'p41', name: 'Victor Osimhen', position: 'ST', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=OSI` },
    { id: 'p43', name: 'Antoine Griezmann', position: 'ST', rating: 88, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GRI` },
    { id: 'p44', name: 'Julian Alvarez', position: 'ST', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ALV` },
    { id: 'p114', name: 'Gabriel Jesus', position: 'ST', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GJE` },
    { id: 'p115', name: 'Alexander Isak', position: 'ST', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ISA` },
    { id: 'p116', name: 'Gerard Moreno', position: 'ST', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=GMO` },
    { id: 'p117', name: 'Borja Mayoral', position: 'ST', rating: 78, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=MAY` },
    { id: 'p118', name: 'Chris Ramos', position: 'ST', rating: 70, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CRA` },
    { id: 'p119', name: 'Sergio Camello', position: 'ST', rating: 65, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CAM` },
    { id: 'p120', name: 'Roberto Fernández', position: 'ST', rating: 55, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RFE` },
    { id: 'p121', name: 'Iker Losada', position: 'ST', rating: 50, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=ILO` },
    // Adding Cristiano Ronaldo
    { id: 'cr7', name: 'Cristiano Ronaldo', position: 'ST', rating: 91, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=CR7` },
    // More players to ensure a broader range
    { id: 'p122', name: 'Endrick', position: 'ST', rating: 75, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=END` },
    { id: 'p148', name: 'Lautaro Martínez', position: 'ST', rating: 87, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=LAU` },
    { id: 'p152', name: 'Jonathan David', position: 'ST', rating: 81, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=DAV` },
    { id: 'p154', name: 'Gonçalo Ramos', position: 'ST', rating: 80, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=RAM` },
    { id: 'p156', name: 'Randal Kolo Muani', position: 'ST', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=KOL` },
    { id: 'p159', name: 'Victor Boniface', position: 'ST', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BON` },
    { id: 'p170', name: 'João Félix', position: 'ST', rating: 82, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=FEL` },
    { id: 'p172', name: 'Ciro Immobile', position: 'ST', rating: 85, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=IMM` },
    { id: 'p179', name: 'Mario Balotelli', position: 'ST', rating: 75, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=BAL` }, // Older player
    { id: 'p194', name: 'Darwin Núñez', position: 'ST', rating: 83, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=NUN` },
    { id: 'p195', name: 'Dusan Vlahovic', position: 'ST', rating: 84, imageUrl: `https://placehold.co/100x100/${getRandomHexColor()}/FFFFFF?text=VLA` },
];


// Definición de las formaciones y sus posiciones específicas
const FORMATIONS = {
    '4-3-3': {
        label: '4-3-3 (Ofensiva)',
        layout: {
            gk: ['GK'],
            defense: ['LB', 'CB1', 'CB2', 'RB'],
            midfield: ['CM1', 'CM2', 'CM3'],
            attack: ['LW', 'ST', 'RW']
        },
        positions: [
            { key: 'GK', label: 'Portero', type: 'GK' },
            { key: 'RB', label: 'Lateral Derecho', type: 'RB' },
            { key: 'CB1', label: 'Defensa Central 1', type: 'CB' },
            { key: 'CB2', label: 'Defensa Central 2', type: 'CB' },
            { key: 'LB', label: 'Lateral Izquierdo', type: 'LB' },
            { key: 'CM1', label: 'Medio Centro 1', type: ['CM', 'CDM'] }, // Puede ser más defensivo
            { key: 'CM2', label: 'Medio Centro 2', type: ['CM', 'CAM'] }, // Más ofensivo
            { key: 'CM3', label: 'Medio Centro 3', type: ['CM', 'CAM'] }, // Más ofensivo
            { key: 'RW', label: 'Extremo Derecho', type: 'RW' },
            { key: 'ST', label: 'Delantero Central', type: 'ST' },
            { key: 'LW', label: 'Extremo Izquierdo', type: 'LW' },
        ],
    },
    '4-4-2': {
        label: '4-4-2 (Clásica)',
        layout: {
            gk: ['GK'],
            defense: ['LB', 'CB1', 'CB2', 'RB'],
            midfield: ['LM', 'CM1', 'CM2', 'RM'],
            attack: ['ST1', 'ST2']
        },
        positions: [
            { key: 'GK', label: 'Portero', type: 'GK' },
            { key: 'RB', label: 'Lateral Derecho', type: 'RB' },
            { key: 'CB1', label: 'Defensa Central 1', type: 'CB' },
            { key: 'CB2', label: 'Defensa Central 2', type: 'CB' },
            { key: 'LB', label: 'Lateral Izquierdo', type: 'LB' },
            { key: 'RM', label: 'Mediocampista Derecho', type: ['RM', 'RW'] },
            { key: 'CM1', label: 'Medio Centro 1', type: ['CM', 'CDM'] },
            { key: 'CM2', label: 'Medio Centro 2', type: ['CM', 'CAM'] },
            { key: 'LM', label: 'Mediocampista Izquierdo', type: ['LM', 'LW'] },
            { key: 'ST1', label: 'Delantero 1', type: 'ST' },
            { key: 'ST2', label: 'Delantero 2', type: 'ST' },
        ],
    },
    '3-5-2': {
        label: '3-5-2 (Dominante)',
        layout: {
            gk: ['GK'],
            defense: ['CB1', 'CB2', 'CB3'],
            midfield: ['LWB', 'CDM1', 'CM1', 'CAM1', 'RWB'],
            attack: ['ST1', 'ST2']
        },
        positions: [
            { key: 'GK', label: 'Portero', type: 'GK' },
            { key: 'CB1', label: 'Defensa Central 1', type: 'CB' },
            { key: 'CB2', label: 'Defensa Central 2', type: 'CB' },
            { key: 'CB3', label: 'Defensa Central 3', type: 'CB' },
            { key: 'RWB', label: 'Carrilero Derecho', type: ['RWB', 'RB', 'RM'] },
            { key: 'CDM1', label: 'Mediocampista Defensivo', type: 'CDM' },
            { key: 'CM1', label: 'Medio Centro', type: ['CM', 'CDM', 'CAM'] },
            { key: 'CAM1', label: 'Medio Ofensivo', type: 'CAM' },
            { key: 'LWB', label: 'Carrilero Izquierdo', type: ['LWB', 'LB', 'LM'] },
            { key: 'ST1', label: 'Delantero 1', type: 'ST' },
            { key: 'ST2', label: 'Delantero 2', type: 'ST' },
        ],
    },
};

// Componente individual de la tarjeta de jugador
const PlayerCard = ({ player, onSelect, isSelected }) => {
    if (!player) return null; // No renderizar si no hay jugador

    const bgColor = isSelected ? 'bg-indigo-600' : 'bg-gray-700';
    const textColor = isSelected ? 'text-white' : 'text-gray-100';
    const buttonBg = isSelected ? 'bg-indigo-800 cursor-not-allowed' : 'bg-indigo-500 hover:bg-indigo-600';
    const buttonText = isSelected ? 'Seleccionado' : 'Seleccionar';

    return (
        <div className={`flex flex-col items-center p-4 rounded-lg shadow-md transition-all duration-300 transform hover:scale-105 ${bgColor} ${textColor} border border-gray-600`}>
            <img
                src={player.imageUrl}
                alt={player.name}
                className="w-24 h-24 rounded-full object-cover mb-3 border-2 border-white shadow-md"
                onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/100x100/CCCCCC/000000?text=${player.name.substring(0, 3).toUpperCase()}`; }} // Fallback image
            />
            <div className="w-16 h-16 rounded-full bg-yellow-400 flex items-center justify-center text-xl font-bold mb-3 text-gray-900 border-2 border-yellow-500">
                {player.rating}
            </div>
            <h3 className="text-lg font-semibold mb-1 text-center">{player.name}</h3>
            <p className="text-sm font-medium mb-3">{player.position}</p>
            <button
                onClick={() => onSelect(player)}
                disabled={isSelected}
                className={`py-2 px-4 rounded-md font-medium text-white ${buttonBg} focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500`}
            >
                {buttonText}
            </button>
        </div>
    );
};

// Componente para la selección de jugadores por posición
const DraftPosition = ({ label, options, onSelectPlayer, selectedPlayer }) => {
    return (
        <div className="mb-6 p-4 bg-gray-800 rounded-lg shadow-inner border border-gray-700">
            <h2 className="text-xl font-bold text-white mb-4 text-center md:text-left">{label}</h2>
            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                {options.map((player) => (
                    <PlayerCard
                        key={player.id}
                        player={player}
                        onSelect={onSelectPlayer}
                        isSelected={selectedPlayer && selectedPlayer.id === player.id}
                    />
                ))}
            </div>
        </div>
    );
};

// Componente principal de la aplicación
export default function App() {
    const [draftedTeam, setDraftedTeam] = useState({});
    const [currentDraftOptions, setCurrentDraftOptions] = useState({});
    const [overallRating, setOverallRating] = useState(0);
    const [draftStarted, setDraftStarted] = useState(false);
    const [selectedFormationKey, setSelectedFormationKey] = useState(null);
    const [currentPositionIndex, setCurrentPositionIndex] = useState(0);
    const [matchResult, setMatchResult] = useState(null);

    // Obtiene la lista de posiciones de la formación actual
    const currentFormationPositions = selectedFormationKey ? FORMATIONS[selectedFormationKey].positions : [];
    const currentPosition = currentFormationPositions[currentPositionIndex];

    // Función para obtener jugadores aleatorios, excluyendo los ya drafteados
    const getRandomPlayers = useCallback((positionTypes, count = 3) => {
        const typesArray = Array.isArray(positionTypes) ? positionTypes : [positionTypes];
        const availablePlayers = ALL_PLAYERS.filter(p => typesArray.includes(p.position));

        const selectedPlayerIds = Object.values(draftedTeam).map(p => p.id);
        const filteredPlayers = availablePlayers.filter(p => !selectedPlayerIds.includes(p.id));

        if (filteredPlayers.length < count) {
            console.warn(`No hay suficientes jugadores disponibles para la posición ${typesArray.join('/')}. Generando menos opciones.`);
            return filteredPlayers.sort(() => 0.5 - Math.random());
        }

        const shuffled = filteredPlayers.sort(() => 0.5 - Math.random());
        return shuffled.slice(0, count);
    }, [draftedTeam]);

    // Lógica para avanzar al siguiente paso del draft
    const goToNextDraftStep = useCallback(() => {
        if (currentPositionIndex < currentFormationPositions.length - 1) {
            setCurrentPositionIndex(prevIndex => prevIndex + 1);
        } else {
            // Draft completado
            console.log("Draft completado!");
        }
    }, [currentPositionIndex, currentFormationPositions.length]);

    // Función para iniciar un nuevo draft con una formación seleccionada
    const startDraftWithFormation = useCallback((formationKey) => {
        setSelectedFormationKey(formationKey);
        setDraftedTeam({});
        setOverallRating(0);
        setCurrentDraftOptions({});
        setDraftStarted(true);
        setMatchResult(null);
        setCurrentPositionIndex(0); // Reset index for new draft

        // Genera las primeras opciones para la primera posición
        const firstPosition = FORMATIONS[formationKey].positions[0];
        if (firstPosition) {
            setCurrentDraftOptions({
                [firstPosition.key]: getRandomPlayers(firstPosition.type, 3)
            });
        }
    }, [getRandomPlayers]);

    // Manejar la selección de un jugador
    const handleSelectPlayer = useCallback((player) => {
        if (!currentPosition) return;

        // Solo permitir selección si la posición actual no está ya ocupada
        if (!draftedTeam[currentPosition.key]) {
            setDraftedTeam(prevTeam => ({
                ...prevTeam,
                [currentPosition.key]: player
            }));
            goToNextDraftStep();
        }
    }, [currentPosition, draftedTeam, goToNextDraftStep]);

    // Efecto para generar nuevas opciones cuando se avanza de posición
    useEffect(() => {
        if (draftStarted && currentPosition && Object.keys(draftedTeam).length <= currentFormationPositions.length) {
            // No generar nuevas opciones si ya se seleccionó el último jugador
            if (Object.keys(draftedTeam).length === currentFormationPositions.length) {
                return;
            }
            setCurrentDraftOptions({
                [currentPosition.key]: getRandomPlayers(currentPosition.type, 3)
            });
        }
    }, [currentPositionIndex, draftStarted, currentPosition, currentFormationPositions.length, getRandomPlayers, draftedTeam]);


    // Calcular la media general del equipo una vez que todos los jugadores han sido drafteados
    useEffect(() => {
        const playersInTeam = Object.values(draftedTeam);
        if (selectedFormationKey && playersInTeam.length === FORMATIONS[selectedFormationKey].positions.length) {
            const totalRating = playersInTeam.reduce((sum, player) => sum + player.rating, 0);
            setOverallRating(Math.round(totalRating / playersInTeam.length));
        } else {
            setOverallRating(0);
        }
    }, [draftedTeam, selectedFormationKey]);

    // Simular un partido
    const simulateMatch = useCallback(() => {
        const opponentRating = Math.floor(Math.random() * (90 - 70 + 1)) + 70; // Oponente entre 70 y 90
        const ratingDifference = overallRating - opponentRating;

        let result;
        if (ratingDifference >= 7) {
            result = '¡VICTORIA DOMINANTE! 🎉';
        } else if (ratingDifference >= 3) {
            result = '¡Victoria! ✅';
        } else if (ratingDifference >= -2 && ratingDifference <= 2) {
            result = '¡Empate! 🤝';
        } else {
            result = '¡Derrota! ❌';
        }
        setMatchResult(result);
    }, [overallRating]);


    // Renderizar la plantilla actual por líneas
    const renderSquadByLine = () => {
        if (!selectedFormationKey) return null;

        const layout = FORMATIONS[selectedFormationKey].layout;
        const gkPlayer = draftedTeam[layout.gk[0]];

        const defensePlayers = layout.defense.map(key => draftedTeam[key]);
        const midfieldPlayers = layout.midfield.map(key => draftedTeam[key]);
        const attackPlayers = layout.attack.map(key => draftedTeam[key]);

        const renderLine = (players, label) => (
            <div className="flex flex-col items-center mb-4">
                <h3 className="text-xl font-bold text-indigo-300 mb-2">{label}</h3>
                <div className="flex flex-wrap justify-center gap-4 w-full">
                    {players.map((player, index) => (
                        player ? (
                            <div key={player.id} className="flex flex-col items-center bg-gray-700 p-3 rounded-lg shadow-sm border border-gray-600 min-w-[100px] text-center">
                                <img
                                    src={player.imageUrl}
                                    alt={player.name}
                                    className="w-16 h-16 rounded-full object-cover mb-1 border border-white"
                                    onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/60x60/CCCCCC/000000?text=${player.name.substring(0, 3).toUpperCase()}`; }}
                                />
                                <p className="text-lg font-bold text-yellow-400">{player.rating}</p>
                                <p className="text-sm text-white truncate w-full">{player.name}</p>
                                <p className="text-xs text-gray-400">{player.position}</p>
                            </div>
                        ) : (
                            <div key={`${label.replace(/\s/g, '-')}-empty-${index}`} className="flex flex-col items-center bg-gray-700 p-3 rounded-lg shadow-sm border border-gray-600 min-w-[100px] text-center">
                                <div className="w-16 h-16 rounded-full bg-gray-500 flex items-center justify-center text-xs text-white">VACÍO</div>
                                <p className="text-xs text-gray-400 mt-2">Pendiente...</p>
                            </div>
                        )
                    ))}
                </div>
            </div>
        );

        return (
            <div className="w-full">
                {gkPlayer && renderLine([gkPlayer], 'Portero')}
                {defensePlayers.some(p => p) && renderLine(defensePlayers, 'Defensa')}
                {midfieldPlayers.some(p => p) && renderLine(midfieldPlayers, 'Mediocampo')}
                {attackPlayers.some(p => p) && renderLine(attackPlayers, 'Delantera')}
            </div>
        );
    };


    return (
        <div className="min-h-screen bg-gradient-to-br from-gray-900 to-black p-6 text-gray-100 flex flex-col items-center">
            <h1 className="text-4xl font-extrabold text-white mb-8 text-center drop-shadow-lg">
                <span className="text-indigo-400">FUT</span> Draft Simulator
            </h1>

            {!draftStarted && (
                <div className="w-full max-w-4xl bg-gray-900 p-6 rounded-xl shadow-2xl border border-gray-700 text-center">
                    <h2 className="text-2xl font-bold text-white mb-6">Selecciona una Formación</h2>
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                        {Object.entries(FORMATIONS).map(([key, formation]) => (
                            <div
                                key={key}
                                onClick={() => startDraftWithFormation(key)}
                                className="bg-gray-800 p-6 rounded-lg shadow-lg cursor-pointer hover:bg-gray-700 transition-all duration-300 transform hover:scale-105 border border-indigo-500 flex flex-col items-center justify-center"
                            >
                                <h3 className="text-xl font-bold text-white mb-2">{formation.label}</h3>
                                <p className="text-gray-300 text-sm">Formación: {key}</p>
                                {/* Mini visual representation of formation can be added here */}
                                <div className="mt-4 grid grid-cols-3 gap-1 w-full text-center">
                                    {/* Simple visual placeholder for positions */}
                                    {key === '4-3-3' && (
                                        <>
                                            <span className="col-span-3 text-lg">FWD</span>
                                            <span className="col-span-3 text-lg">MID</span>
                                            <span className="col-span-3 text-lg">DEF</span>
                                            <span className="col-span-3 text-lg">GK</span>
                                        </>
                                    )}
                                     {key === '4-4-2' && (
                                        <>
                                            <span className="col-span-3 text-lg">FWD</span>
                                            <span className="col-span-3 text-lg">MID</span>
                                            <span className="col-span-3 text-lg">DEF</span>
                                            <span className="col-span-3 text-lg">GK</span>
                                        </>
                                    )}
                                     {key === '3-5-2' && (
                                        <>
                                            <span className="col-span-3 text-lg">FWD</span>
                                            <span className="col-span-3 text-lg">MID</span>
                                            <span className="col-span-3 text-lg">DEF</span>
                                            <span className="col-span-3 text-lg">GK</span>
                                        </>
                                    )}
                                </div>
                            </div>
                        ))}
                    </div>
                </div>
            )}

            {draftStarted && (
                <>
                    <button
                        onClick={() => {
                            setDraftStarted(false);
                            setSelectedFormationKey(null);
                            setMatchResult(null);
                        }}
                        className="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-6 rounded-full shadow-md transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 focus:ring-offset-gray-900 mb-8"
                    >
                        Reiniciar Draft
                    </button>

                    {currentPosition && (
                        <div className="w-full max-w-4xl bg-gray-900 p-6 rounded-xl shadow-2xl mb-8 border border-gray-700">
                            <h2 className="text-2xl font-bold text-white mb-4 text-center">
                                Paso {currentPositionIndex + 1} de {currentFormationPositions.length}: Selecciona tu {currentPosition.label}
                            </h2>
                            <DraftPosition
                                label={currentPosition.label}
                                options={currentDraftOptions[currentPosition.key] || []}
                                onSelectPlayer={handleSelectPlayer}
                                selectedPlayer={draftedTeam[currentPosition.key]}
                            />
                        </div>
                    )}

                    {Object.keys(draftedTeam).length === currentFormationPositions.length && (
                        <div className="w-full max-w-4xl bg-green-800 p-6 rounded-xl shadow-2xl mb-8 border border-green-600 text-center">
                            <h2 className="text-3xl font-extrabold text-white mb-4">
                                ¡Draft Completado!
                            </h2>
                            <p className="text-xl text-white mb-4">
                                Media General de tu Equipo: <span className="font-bold text-4xl">{overallRating}</span>
                            </p>
                            <button
                                onClick={simulateMatch}
                                className="mt-4 bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition-all duration-300 transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-blue-500 focus:ring-opacity-75"
                            >
                                Jugar Partido
                            </button>
                            {matchResult && (
                                <p className="text-3xl font-bold mt-4 animate-pulse">
                                    Resultado: {matchResult}
                                </p>
                            )}
                        </div>
                    )}

                    {selectedFormationKey && (
                        <div className="w-full max-w-4xl bg-gray-900 p-6 rounded-xl shadow-2xl border border-gray-700">
                            <h2 className="text-2xl font-bold text-white mb-4 text-center">Tu Plantilla ({FORMATIONS[selectedFormationKey].label})</h2>
                            {renderSquadByLine()}
                        </div>
                    )}
                </>
            )}
        </div>
    );
}
npm run build o yarn build
