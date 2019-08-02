## 1 -  Save e Update em um único método

**#CodigoDuplicado**

### Motivo: 

>  Esse código de save e de update é identico como em 99% dos casos de lógcioa de save/update. Então deveria ser um método só.



### Forma Anterior:

```c#
public void SaveFromForm(InfoForm formDados, long id)
		{
			EntityBean bean = new EntityBean();

			bean.Id = id;
			bean.value1 = formDados.value1;
			bean.value2 = formDados.value2;
			bean.value3 = formDados.value3;
			bean.value4 = formDados.value4.HasValue ? (long?)formDados.value4.Value.TotalSeconds : null;
			bean.value5 = formDados.value5;
			bean.value6 = formDados.value6;

			T2EstadiaDao dao = GetDao();
			dao.Save(bean);
		}//func

		public void UpdateFromForm(T2EstadiaInfoForm form, T2Estadia bean)
		{
			bean.value1 = form.value1;
			bean.value2 = form.value2;
			bean.value3 = form.value3;
			bean.value4 = form.value4.HasValue ? (long?)form.value4.Value.TotalSeconds : null;
			bean.value5 = form.value5;
			bean.value6 = form.value6;

			bean.LastUpdate = DateTime.Now;

			EntityBeanDoa dao = GetDao();
			dao.Update(bean);
		}//func
```



### Correnção:

```c#
public void SaveOrUpdateFromForm(InfoForm form, EntityBean beanRaw, long id)
		{
			bool isInsert = beanRaw == null;
			EntityBean bean;
			if (isInsert)
			{
				bean = new EntityBean();
				bean.Id = id;
			}//if
			else
			{
				bean = beanRaw;
			}//else

			bean.value1 = form.value1;
			bean.value2 = form.value2;
			bean.value3 = form.value3;
			bean.value4 = form.value4.HasValue ? (long?)form.value4.Value.TotalSeconds : null;
			bean.value5 = form.value5;
			bean.value6 = form.value6;

			bean.LastUpdate = DateTime.Now;

			EntityBeanDao dao = GetDao();
			if (isInsert)
			{
				dao.Save(bean);
			}//if
			else
			{
				dao.Update(bean);
			}//else
		}//func
```