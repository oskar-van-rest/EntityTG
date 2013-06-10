If GMF models are lost: regenerate them from the Ecore using EuGENia and
- EntityLang.gmfgraph:
     - set margin border of AttributeFigure to insets 0,0,0,5
     - remove border
     - set fill of attribute rectangle to false
- add Default Size Attributes 10,10 to EntityLang.gmfgen -> Gen Editor Generator EntityTG.Diagram -> Gen Diagram ModuleEditPart -> Gen Child Node AttributeEditPart -> Inner Class Viewmap AttributeFigure

 - Change 'label mapping' into 'expression label mapping' with the following OCL View Expressions:
if type->isEmpty() then id else id.concat(' : '.concat(type.id)) endif


- For direct editing of attributes:

	/**
	 * @generated NOT
	 */
	private IStatus updateValues(EObject target, String newString) throws ExecutionException {
		Attribute a = (Attribute) target;
		
		String name = newString.split(":")[0].trim();
		a.setId(name);

		a.setType(null);
		if (newString.split(":").length > 1) {
			String type = newString.split(":")[1].trim();
			Module m = (Module) a.eContainer().eContainer();
			Iterator<Type> it = m.getTypes().iterator();
			while (it.hasNext()) {
				Type t = it.next();
				if (t instanceof DataType && ((DataType) t).getId().equals(type)) {
					a.setType((DataType) t);
				}
			}
		}

		return Status.OK_STATUS;
	}