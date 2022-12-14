# Project-R
# College work on the characterization of a library of chemical compounds
# Library where the work consists--------> https://hmdb.ca/

!pip install rdkit
!pip install pandas 
import pandas as pd
import numpy as np
from rdkit import Chem
from rdkit.Chem.Lipinski import RingCount
from rdkit.Chem.Lipinski import NumHAcceptors
from rdkit.Chem.Lipinski import NumHDonors
from rdkit.Chem.Lipinski import NumRotatableBonds
from rdkit.Chem import Descriptors
from rdkit.Chem import PandasTools
from rdkit.Chem import Descriptors, Crippen, Lipinski, MolSurf, rdMolDescriptors, MACCSkeys
from rdkit.Chem.Descriptors import ExactMolWt
from rdkit.Chem.Descriptors import TPSA
from rdkit.Chem.Crippen import MolLogP
from rdkit.Chem.ChemUtils import SDFToCSV
import csv
from rdkit import Chem, RDLogger
from rdkit.Chem import Descriptors, MolSurf, rdMolDescriptors, AllChem, Draw
from google.colab import drive
drive.mount('/content/drive')

from google.colab import data_table
data_table.enable_dataframe_formatter()

structures = '/content/drive/MyDrive/structures.sdf'
df = PandasTools.LoadSDF(structures)

df.head()

df.info()

### Definition the suppl
suppl = Chem.SDMolSupplier('/content/drive/MyDrive/structures.sdf')
n_mols = len(suppl)

i = suppl[0]
print(i.GetNumAtoms())

teste = i.GetPropNames()
for j in teste:
    print(j)

for j in suppl:
  print(ExactMolWt(j))

print(Descriptors.ExactMolWt(i))
### Smile
m = Chem.MolFromMolFile(structures)
Chem.MolToSmiles(m)

### NumHAcceptors
Lipinski.NumHAcceptors(m)

### NumHDonors
Lipinski.NumHDonors(m)

### NumRotatableBonds
Lipinski.NumRotatableBonds(m)

### TPSA
Descriptors.TPSA(m)

### MolLogP
Descriptors.MolLogP(m)

### RingCount
Lipinski.RingCount(m)

### Calculation of the Descriptors of each Molecule
all_mols = Chem.SDMolSupplier(structures)

Smiles = []
NumAtoms = []
ExactMolWt = []
MolLogP = []
NumHAcceptors = []
NumHDonors = []
Ring_Count = []
TPSA = []
NumRotatableBonds = []
Molecular_Formula = []
for x in range(0, len(all_mols)):
  if all_mols[x] is None:continue
  NumAtoms.append(all_mols[x].GetNumAtoms())
  Smiles.append(Chem.MolToSmiles(all_mols[x]))
  ExactMolWt.append(Descriptors.ExactMolWt(all_mols[x]))
  NumHAcceptors.append(Lipinski.NumHAcceptors(all_mols[x]))
  NumHDonors.append(Lipinski.NumHDonors(all_mols[x]))
  MolLogP.append(Crippen.MolLogP(all_mols[x]))
  Molecular_Formula.append(rdMolDescriptors.CalcMolFormula(all_mols[x]))
  Ring_Count.append(Lipinski.RingCount(all_mols[x]))
  NumRotatableBonds.append(Lipinski.NumRotatableBonds(all_mols[x]))
  TPSA.append(Descriptors.TPSA(all_mols[x]))
  
  all_mols = Chem.SDMolSupplier(structures)

### Converting to CSV 
Smiles = []
NumAtoms = []
ExactMolWt = []
MolLogP = []
NumHAcceptors = []
NumHDonors = []
Ring_Count = []
TPSA = []
NumRotatableBonds = []
Molecular_Formula = []
for x in range(0, len(all_mols)):
  if all_mols[x] is None:continue
  NumAtoms.append(all_mols[x].GetNumAtoms())
  Smiles.append(Chem.MolToSmiles(all_mols[x]))
  ExactMolWt.append(Descriptors.ExactMolWt(all_mols[x]))
  NumHAcceptors.append(Lipinski.NumHAcceptors(all_mols[x]))
  NumHDonors.append(Lipinski.NumHDonors(all_mols[x]))
  MolLogP.append(Crippen.MolLogP(all_mols[x]))
  Molecular_Formula.append(rdMolDescriptors.CalcMolFormula(all_mols[x]))
  Ring_Count.append(Lipinski.RingCount(all_mols[x]))
  NumRotatableBonds.append(Lipinski.NumRotatableBonds(all_mols[x]))
  TPSA.append(Descriptors.TPSA(all_mols[x]))
  
  Smart_ID = df.DATABASE_ID
  
  i = {'df.DATABASE_ID': Smart_ID,
     'Smiles': Smiles,
    'Molecular_Formula': Molecular_Formula,
    'NumAtoms': NumAtoms,
    'ExactMolWt': ExactMolWt,
    'NumRotatableBonds': NumRotatableBonds,
    'MolLogP': MolLogP,
    'Ring_Count': Ring_Count,
    'NumHAcceptors': NumHAcceptors,
    'TPSA': TPSA,
    'NumHDonors': NumHDonors,

}

pd.DataFrame(i).to_csv('structures', sep = ';')

Particles_Description = pd.DataFrame(data=i)

Particles_Description
