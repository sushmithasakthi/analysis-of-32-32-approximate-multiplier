module multiplier_32x32(
    input [31:0] a,
    input [31:0] b,
    output reg [63:0] product
);

    reg [31:0] A, Q, M;
    reg Qn_1;
    integer i;

    always @ (a or b) begin
        A = 0;
        Q = b;
        M = a;
        Qn_1 = 0;
        for (i = 0; i < 32; i = i + 1) begin
            case ({Q[0], Qn_1})
                2'b01: A = A + M;
                2'b10: A = A - M;
                default: ;
            endcase
            Qn_1 = Q[0];
            Q = {A[31], Q[31:1]};
            A = {A[31], A[31:1]};
        end

        product = {A, Q};
    end

endmodule
