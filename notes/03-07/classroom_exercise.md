    static void visitBlock(struct Block *node) {
      visitVarDecls(node->vardecls);

      // skip past function definitions, since we want to start running the current function body
      int saved_jump = emit(VM_BR(0));
      visitFuncDecls(node->funcdecls);

      // code_index is now the body of the function (or main)
      int entry_point = code_index;
      backpatch(saved_jump, code_index - saved_jump); // backpatch
      visitStatement(node->statement);
    }
