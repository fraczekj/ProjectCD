Lab 4: Revision 2

import java.util.HashMap;
import java.util.Map;

public class AST {
	public interface Node
	{
		<T> T accept (Visitor<T> v);
	}
	public interface Visitor<T>
	{
		T visit (Command command);
		T visit (Arguments arguments);
		T visit (Block block);
		T visit (Id id);
	}
	public interface Statement extends Node {}
	public interface Expression extends Node {}
	
	public static class Id implements Expression {
		String id;
		public Id (String id) { this.id = id; }
		public <T> T accept(Visitor<T> v) { return v.visit(this); }		
	}
	public static Id id (String id) { return new Id (id); }
	
	public static class Command implements Statement {
		Id type;
		Arguments arguments = null;
		
		public Command (Id type)
		{
			this.type = type;
		}
		public Command (Id type, Arguments arguments)
		{
			this.type = type;
			this.arguments = arguments;
		}
		
		public <T> T accept(Visitor<T> v) { return v.visit(this); }		
	}
	public static Command command (Id type) { return new Command (type); }
	public static Command command (Id type, Arguments arguments) { return new Command (type, arguments); }

	public static class Arguments implements Expression {
		String s;
		public Arguments (String s) { this.s = s; }
		public <T> T accept(Visitor<T> v) { return v.visit(this); }		
	}
	public static Arguments arguments (String s) { return new Arguments(s); }

	public static class Block implements Statement {
		Statement[] statements;
		public Block (Statement ... statements) { this.statements = statements; }
		public <T> T accept(Visitor<T> v) { return v.visit(this); }		
	}
	public static Block block (Statement... statements) { return new Block (statements); }

	public static class ExpressionInterpreter implements Visitor<Integer>
	{
		Map<String, Integer> symbols;
		public ExpressionInterpreter(Map<String, Integer> symbols) {
			this.symbols = symbols;
		}
		public Integer visit(Id id) 
		{
			if (symbols.containsKey(id.id))
				return symbols.get(id.id);
			else return 0;
		}
		
		public Integer visit(Command command) { return null; }
		public Integer visit(Arguments arguments) { return null; }
		public Integer visit(Block block) { return null; }
	}
	public static class StatementInterpreter implements Visitor<Void>
	{
		Map<String,Integer> symbols = new HashMap<String, Integer>();
		ExpressionInterpreter eval = new ExpressionInterpreter(symbols);
		
		public Void visit(Command command)
		{
			System.out.println("Command: " + command.type.id);
			if(command.arguments != null)
			{
			System.out.println("Arguments: " + command.arguments.s);
			}
			return null;
		}
		
		public Void visit(Arguments arguments)
		{
			return null;
		}
		
		public Void visit(Block block) {
			for (Statement s : block.statements)
				s.accept(this);
			return null;
		}
		
		public Void visit(Id id) { return null; }
		public Void visit(Number num) { return null; }
	}
	
	public static void main (String[] args)
	{
		Node program = block(command(id("TEST")),command(id("CONNECT"),arguments("255.255.255.255:80")));
		StatementInterpreter runner = new StatementInterpreter();
		program.accept(runner);
		
		for (String s : runner.symbols.keySet())
			System.out.format("%s: %d\n", s, runner.symbols.get(s));
	}
}