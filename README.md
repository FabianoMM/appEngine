appEngine
=========

Objectfy-appengine, DML commands


package orcamentoPessoal.server;

import java.util.ArrayList;
import java.util.Map;

import orcamentoPessoal.client.GreetingService;
import orcamentoPessoal.client.entities.Item;
import orcamentoPessoal.client.entities.Pessoa;
import orcamentoPessoal.client.entities.Produto;

import com.google.appengine.api.datastore.DatastoreService;
import com.google.appengine.api.datastore.Transaction;
import com.google.gwt.user.server.rpc.RemoteServiceServlet;
import com.googlecode.objectify.Key;
import com.googlecode.objectify.NotFoundException;
import com.googlecode.objectify.Objectify;
import com.googlecode.objectify.ObjectifyFactory;
import com.googlecode.objectify.ObjectifyService;
import com.googlecode.objectify.Query;
import com.smartgwt.client.docs.Date;

/**
 * The server side implementation of the RPC service.
 */
@SuppressWarnings("serial")
public class GreetingServiceImpl extends RemoteServiceServlet implements
		GreetingService {

	public void persistentPessoa(String name, String email, String register, String user)
	{
		ObjectifyService.register(Pessoa.class);   //Registers new entity within the service
		Objectify obj = ObjectifyService.begin();  // open transaction
		Pessoa pessoa = new Pessoa();
		pessoa.setNome(name);
		pessoa.setEmail(email);
		pessoa.setRG(register);
		pessoa.setUsuario(user);
		
		obj.put(pessoa);                          //Add new record person
	}
	
	public void persistentProduto(String CodProd, String DescProd, String PrecoProd, String LocalProd)
	{
		ObjectifyService.register(Produto.class);
		Objectify objprod = ObjectifyService.begin();
		Produto produto = new Produto();
		produto.setCodProd(CodProd);
		produto.setDescProd(DescProd);
		produto.setPrecoProd(PrecoProd);
		produto.setLocalProd(LocalProd);
		
		objprod.put(produto);
	}
	
	public ArrayList<Produto> searchProduto()
	{
		ObjectifyService.register(Produto.class);   // ||
		Objectify objcprod = ObjectifyService.begin(); // ||
		
		 Query<Produto> query = objcprod.query(Produto.class); //New query in entity produto
		 
		 ArrayList<Produto> produtos = new ArrayList<Produto>();
		 
		 for (Produto produto : query) {
			produtos.add(produto);			//add in generic list
		}
		 
		 return produtos;              		
	}
	
	public ArrayList<Pessoa> searchPessoa()
	{
		ObjectifyService.register(Pessoa.class);
		Objectify objc = ObjectifyService.begin();
		
		 Query<Pessoa> query = objc.query(Pessoa.class);
		 
		 ArrayList<Pessoa> pessoas = new ArrayList<Pessoa>();
		 
		 for (Pessoa pessoa : query) {
			pessoas.add(pessoa);
		}
		 
		 return pessoas;
	}

	public void persistentItem(Long pessoa, Long produto, String qtde, String total, String descricaoProd)
	{
		ObjectifyService.register(Item.class);
		Objectify obj = ObjectifyService.begin();
		Item item = new Item();
		item.setDescricaoProduto(descricaoProd);
		item.setIdProduto(produto);
		item.setQtde(qtde);
		item.setTotal(total);
		item.setIdPessoa(pessoa);
		
		obj.put(item);
	}
	
	
	public void deletePessoa(Pessoa person)
	{
		ObjectifyService.register(Pessoa.class);   // ||
		Objectify obj = ObjectifyService.begin();   // ||
		obj.delete(person);								//Delete object passed by parameter
	}
}
