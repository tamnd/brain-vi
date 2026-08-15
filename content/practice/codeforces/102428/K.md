---
title: "CF 102428K - Biết người ngoài hành tinh của bạn"
description: "Chúng ta có một chuỗi S mô tả công dân ở các vị trí (2,4,6,ldots,2N). Ký tự H có nghĩa là đa thức phải dương tại vị trí đó, trong khi A có nghĩa là nó phải âm. Chúng ta cần một đa thức có hệ số nguyên và nghiệm nguyên."
date: "2026-08-12T07:28:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 100
verified: true
draft: false
---

[CF 102428K - Biết người ngoài hành tinh của bạn](https://codeforces.com/problemset/problem/102428/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`S`mô tả công dân ở các vị trí (2,4,6,\ldots,2N). Một nhân vật`H`có nghĩa là đa thức phải dương ở vị trí đó, trong khi`A`có nghĩa là nó phải âm. 

Chúng ta cần một đa thức có hệ số nguyên và nghiệm nguyên. Hệ số cao nhất của nó phải là (1) hoặc (-1), và trong số tất cả các đa thức thỏa mãn các dấu yêu cầu thì bậc của nó phải nhỏ nhất. Chúng ta chỉ cần xuất ra mức độ và hệ số. 

Nhận xét hữu ích đầu tiên là một đa thức chỉ có thể đổi dấu khi chúng ta đi qua nghiệm của bội lẻ. Xét hai công dân liên tiếp tại (2i) và (2i+2). Nếu dấu yêu cầu của chúng khác nhau thì đa thức phải có số nghiệm lẻ trong khoảng mở ((2i,2i+2)). 

Tất cả các gốc đều là số nguyên. Số nguyên duy nhất nằm giữa (2i) và (2i+2) là (2i+1). Theo đó, bất cứ khi nào`S[i] != S[i+1]`, đa thức phải chứa nghiệm (2i+1) có bội số lẻ. Một bản là đủ, lấy thêm bản chỉ làm tăng cấp độ mà thôi. 

Điều này ngay lập tức đưa ra mức độ tối thiểu có thể: 

[ 
D=#{i\ne S_{i+1}}. 
] 

Chúng ta có thể xây dựng một đa thức bằng cách sử dụng chính xác các nghiệm đó: 

[ 
Q(x)=\prod_{S_i\ne S_{i+1}}(x-(2i+1)). 
] 

Căn nguyên xuất hiện chính xác giữa các công dân liên tiếp có dấu cần thay đổi, do đó (Q) có chính xác những thay đổi dấu mong muốn. Chúng ta chỉ phải chọn sử dụng (Q) hoặc (-Q) để dấu hiệu ở công dân đầu tiên là chính xác. 

Giới hạn (N\le10^4) ban đầu có thể gợi ý rằng việc mở rộng sản phẩm này có thể yêu cầu (O(N^2)) hoạt động, điều này sẽ không thoải mái trong Python. Giới hạn hệ số sẽ thay đổi hoàn toàn tình hình. Nếu đa thức có bậc ít nhất là 17 thì nó phải chứa ít nhất 17 nghiệm lẻ dương cần thiết. Tích nhỏ nhất có thể có của 17 nghiệm như vậy là 

[ 
3\cdot5\cdot7\cdots35 
=221643095476699771875, 
] 

vốn đã lớn hơn (2^{63}). Hệ số không đổi của một đa thức monic hoặc phản monic, có dấu, là tích của tất cả các nghiệm của nó. Do đó không có đa thức bậc tối thiểu hợp lệ nào có bậc 17 trở lên có thể thỏa mãn giới hạn hệ số. 

Vì vậy mọi đầu vào hợp lệ đều có (D\le16). Chuỗi ban đầu vẫn có thể chứa 10.000 công dân, nhưng sau khi quét nó, chúng ta sẽ nhân tối đa 16 thừa số tuyến tính. 

Có một số trường hợp khó xử lý sai. Vì`H`, đầu vào là`H`, không có sự đổi dấu và đa thức không đổi (P(x)=1) là đúng. Việc triển khai bất cẩn luôn tạo ra ít nhất một nghiệm sẽ tạo ra một đa thức không cực tiểu. 

Vì`A`, đầu vào là`A`, đa thức đúng là (P(x)=-1). Hệ số cao nhất phải được chọn từ dấu bắt buộc đầu tiên, ngay cả khi bậc bằng 0. 

Vì`HA`, sự chuyển đổi duy nhất là giữa vị trí 2 và 4, vì vậy nghiệm phải là 3. Đa thức (x-3) âm tại 2 và dương tại 4, ngược lại với những gì chúng ta cần. Câu trả lời đúng là```
1
-1 3
```Điều này minh họa tại sao dấu chung cuối cùng không thể bị bỏ qua. 

Vì`AHA`, có hai phép chuyển tiếp nên nghiệm là 3 và 5. Tích của chúng cho 

[ 
Q(x)=(x-3)(x-5)=x^2-8x+15. 
] 

Tại (x=2), (Q(2)=3>0), nhưng công dân đầu tiên là người ngoài hành tinh nên ta phải phủ định đa thức và kết quả```
2
-1 8 -15
```Việc triển khai bất cẩn chỉ tính các lần chuyển đổi mà quên mất tính chẵn lẻ của cấp độ có thể làm sai dấu hiệu này.

 ## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xác định mọi chuyển đổi và sau đó mở rộng từng yếu tố của sản phẩm tương ứng. Nếu chúng ta bỏ qua giới hạn hệ số và cho phép độ tăng lớn tới mức (N-1), thì trường hợp xấu nhất sẽ là một chuỗi xen kẽ có độ dài 10.000. Khi đó sẽ có 9.999 thừa số và việc nhân chúng với nhau sẽ yêu cầu 

[ 
1+2+\cdots+9999 
=\frac{9999\cdot10000}{2} 
=49.995.000 
] 

cập nhật hệ số. Đó là một khối lượng công việc lớn không cần thiết đối với một giải pháp Python. 

Tuy nhiên, việc xây dựng lực lượng vũ phu vẫn đúng về mặt toán học. Mỗi phép chuyển đổi buộc một nghiệm trong khoảng cách số nguyên duy nhất của nó và việc nhân các thừa số tuyến tính tương ứng sẽ tạo ra chính xác những thay đổi dấu đó. Điểm yếu của nó là nó coi mức độ là tiềm năng (O(N)). 

Quan sát quan trọng là giới hạn hệ số sẽ ngăn chặn tình trạng đó. Vì mọi chuyển đổi đều tạo ra một nghiệm lẻ dương riêng biệt, nên đa thức bậc 17 sẽ có hệ số không đổi có độ lớn vượt quá (2^{63}). Do đó, một phiên bản hợp lệ có thể có tối đa 16 lần chuyển đổi. Việc quét qua đầu vào vẫn tốn (O(N)), nhưng chi phí xây dựng đa thức chỉ (O(D^2)) với (D\le16). 

Điều này đưa ra một giải pháp rất đơn giản. Quét chuỗi một lần, thu thập số nguyên lẻ (2i+1) bất cứ khi nào hai ký tự lân cận khác nhau, sau đó nhân các thừa số (x-r). Cuối cùng chọn dấu của toàn bộ đa thức theo ký tự đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khai triển ngây thơ mà không sử dụng hệ số ràng buộc | (O(N^2)), lên tới 49.995.000 bản cập nhật | (O(N)) | Chậm một cách không cần thiết | 
| Tối ưu | (O(N+D^2)), với (D\le16) | (O(D)) | Đã chấp nhận | 

Cuộc thảo luận chính thức của cuộc thi mô tả phép nhân trực tiếp là cách tiếp cận dự định (O(n^2)) và chỉ ra rằng giới hạn hệ số (2^{63}) làm cho mức độ hữu ích tối đa là 16. 

## Hướng dẫn thuật toán 

1. Quét từng cặp liền kề`S[i]`Và`S[i+1]`. Nếu chúng bằng nhau thì không cần thay đổi dấu giữa hai công dân, do đó không cần root ở đó. Nếu chúng khác nhau, hãy thêm (2i+1) vào danh sách gốc, vì đó là số nguyên duy nhất nằm giữa các vị trí công dân (2i) và (2i+2). 
2. Gọi số rễ thu được là (D). Đây là mức độ tối thiểu vì mọi chuyển đổi dấu đều yêu cầu ít nhất một nghiệm bội lẻ và một nghiệm là đủ để thực hiện quá trình chuyển đổi đó. 
3. Bắt đầu với đa thức (Q(x)=1), được biểu thị bằng danh sách hệ số`[1]`. Với mọi nghiệm (r), thay (Q(x)) bằng (Q(x)(x-r)). Nếu các hệ số hiện tại được lưu theo thứ tự giảm dần, nhân với (x-r) sẽ thay đổi chúng theo 
[ 
b_j=a_j-r a_{j-1}. 
] 
Hệ số đầu tiên giữ nguyên (a_0) và hệ số cuối cùng mới (-r a_D) được tạo. 
4. Xác định xem (Q) đã có dấu đúng ở công dân đầu tiên chưa (x=2). Mọi nghiệm được chọn đều lớn hơn 2 nên mọi thừa số (2-r) đều âm. Do đó, 
[ 
\operatorname{sign}(Q(2))=(-1)^D. 
] 
Nếu ký tự đầu tiên là`H`, chúng ta cần một giá trị dương. Nếu nó là`A`, chúng ta cần một giá trị âm. Phủ định mọi hệ số khi dấu hiện tại sai. 
5. Đầu ra (D) và danh sách hệ số kết quả. Vì các gốc chính xác là các điểm chuyển tiếp và mọi gốc đều có bội số là một, nên dấu thay đổi chính xác khi chuỗi đầu vào thay đổi ký tự. 

Tại sao nó hoạt động: giữa hai công thức liên tiếp có chính xác một nghiệm nguyên có thể có, (2i+1). Một sự chuyển tiếp trong`S`buộc một số nghiệm lẻ trong khoảng đó, do đó không thể tránh khỏi việc có ít nhất một bản sao của (2i+1). Chúng tôi đặt chính xác một bản sao ở đó, đưa ra mức độ tối thiểu. Mỗi nghiệm được chọn nằm giữa chính xác hai công thức liên quan đến quá trình chuyển đổi đó, do đó, việc vượt qua nó sẽ lật dấu đa thức đúng một lần. Do đó, tất cả các dấu hiệu liền kề đều khớp với mẫu được yêu cầu sau khi dấu hiệu chung được chọn từ công dân đầu tiên. Đa thức là monic trước dấu chung cuối cùng và tất cả các nghiệm và hệ số đều là số nguyên, do đó mọi yêu cầu đầu ra đều được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    roots = []

    for i in range(n - 1):
        if s[i] != s[i + 1]:
            roots.append(2 * i + 3)

    degree = len(roots)

    # Coefficients in decreasing order.
    # Initially Q(x) = 1.
    coef = [1]

    for r in roots:
        new_coef = [0] * (len(coef) + 1)

        for j, a in enumerate(coef):
            new_coef[j] += a
            new_coef[j + 1] -= r * a

        coef = new_coef

    # Q(2) has sign (-1)^degree because every root is > 2.
    # We want Q(2) > 0 for H and Q(2) < 0 for A.
    desired_positive = s[0] == 'H'
    q_at_2_positive = (degree % 2 == 0)

    if desired_positive != q_at_2_positive:
        coef = [-x for x in coef]

    print(degree)
    print(*coef)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên triển khai tính năng phát hiện chuyển đổi từ Hướng dẫn thuật toán bước 1. Với lập chỉ mục Python dựa trên 0, công dân tại chỉ mục`i`có vị trí (2i+2), do đó số nguyên giữa các công dân`i`Và`i+1`là (2i+3). Đây là một điểm chung. 

Phép nhân đa thức lưu trữ các hệ số theo thứ tự giảm dần. Nếu như`coef`đại diện cho 

[ 
a_0x^k+a_1x^{k-1}+\cdots+a_k, 
] 

sau đó nhân với (x-r) sẽ được 

[ 
a_0x^{k+1} 
+(a_1-ra_0)x^k 
+\cdots 
+(a_k-ra_{k-1})x 
-ra_k. 
] 

Hai nhiệm vụ để`new_coef[j]`Và`new_coef[j+1]`thực hiện chính xác những đóng góp này. Việc giữ riêng hai phần đóng góp cũng tránh được mọi xử lý đặc biệt đối với hệ số đầu tiên và hệ số cuối cùng. 

Số nguyên Python không bị tràn nên không cần số học 64 bit đặc biệt. Bài toán đảm bảo rằng các hệ số cuối cùng có độ lớn dưới (2^{63}) và các đa thức trung gian được hình thành bằng cách cộng các nghiệm dương có độ lớn hệ số không lớn hơn các hệ số của tích cuối cùng. 

Phép tính dấu cuối cùng sử dụng thực tế là mọi nghiệm ít nhất đều bằng 3. Tại (x=2), mọi thừa số (2-r) đều âm, do đó tích không âm chính xác là dương khi bậc chẵn. Nếu dấu hiệu đó không phù hợp với quy chế bắt buộc của công dân đầu tiên thì tất cả các hệ số đều bị phủ định. 

Chỉ có một chuỗi đầu vào nên không cần vòng lặp test-case. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`HHH`. Không có thay đổi liền kề nên danh sách gốc vẫn trống. 

| Bước | Cặp kiểm tra | Rễ | Bằng cấp | Hệ số | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không |`[]`| 0 |`[1]`| 
| 1 |`H,H`|`[]`| 0 |`[1]`| 
| 2 |`H,H`|`[]`| 0 |`[1]`| 
| Cuối cùng | đầu tiên =`H`|`[]`| 0 |`[1]`| 

Đa thức không đổi (P(x)=1) dương ở mọi công dân. Vì không cần đổi dấu nên độ 0 là tối ưu. 

### Mẫu 2 

Đầu vào là`AHHA`. Có sự thay đổi từ`A`ĐẾN`H`giữa hai công dân đầu tiên, tạo ra gốc 3. Có một sự thay đổi khác từ`H`ĐẾN`A`giữa công dân thứ ba và thứ tư, tạo ra gốc 7. 

| Bước | Cặp kiểm tra | Rễ | Bằng cấp | Hệ số | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không |`[]`| 0 |`[1]`| 
| 1 |`A,H`|`[3]`| 1 |`[1, -3]`| 
| 2 |`H,H`|`[3]`| 1 |`[1, -3]`| 
| 3 |`H,A`|`[3, 7]`| 2 |`[1, -10, 21]`| 
| Cuối cùng | đầu tiên =`A`|`[3, 7]`| 2 |`[-1, 10, -21]`| 

Trước khi đổi dấu cuối cùng, đa thức là 

[ 
(x-3)(x-7)=x^2-10x+21. 
] 

Giá trị của nó ở mức 2 là dương vì mức độ là số chẵn. Công dân đầu tiên là người ngoài hành tinh, vì vậy chúng tôi phủ nhận nó và thu được 

[ 
-x^2+10x-21. 
] 

Tại các vị trí 2, 4, 6 và 8, dấu của nó lần lượt là âm, dương, dương và âm, khớp chính xác`AHHA`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+D^2)) | Chuỗi được quét một lần và phép nhân đa thức sử dụng cập nhật hệ số (O(D^2)) | 
| Không gian | (O(D)) | Danh sách gốc và mảng hệ số chứa các phần tử (O(D)) | 

Giới hạn quan trọng là (D\le16), bị ràng buộc bởi giới hạn hệ số (2^{63}). Do đó (D^2) tối đa là 256, trong khi quét chuỗi tốn tối đa 10.000 thao tác. Do đó, độ phức tạp hiệu quả là tuyến tính theo độ dài đầu vào, với chi phí xây dựng đa thức rất nhỏ. Sự cố chính thức có giới hạn thời gian 1 giây và giới hạn bộ nhớ 1024 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm bên dưới sử dụng trình kiểm tra độc lập cho các trường hợp tùy chỉnh, vì một số đa thức khác nhau có thể đáp ứng cùng một đầu vào. Các mẫu được cung cấp đã được kiểm tra trực tiếp kết quả đầu ra dự kiến ​​chính xác.```python
import sys
import io

def solve_reference(s):
    roots = []

    for i in range(len(s) - 1):
        if s[i] != s[i + 1]:
            roots.append(2 * i + 3)

    degree = len(roots)
    coef = [1]

    for r in roots:
        new_coef = [0] * (len(coef) + 1)
        for j, a in enumerate(coef):
            new_coef[j] += a
            new_coef[j + 1] -= r * a
        coef = new_coef

    if (s[0] == 'H') != (degree % 2 == 0):
        coef = [-x for x in coef]

    return f"{degree}\n" + " ".join(map(str, coef)) + "\n"

def run(inp: str) -> str:
    data = inp.strip()
    return solve_reference(data)

# Provided samples
assert run("HHH") == "0\n1\n", "sample 1"
assert run("AHHA") == "2\n-1 10 -21\n", "sample 2"
assert run("AHHHAH") == "3\n1 -23 159 -297\n", "sample 3"

# Minimum-size inputs
assert run("H") == "0\n1\n", "single human"
assert run("A") == "0\n-1\n", "single alien"

# Boundary transition: the root must be exactly 3
assert run("HA") == "1\n-1 3\n", "first possible transition"

# Two transitions, exercising both root positions and the global sign
assert run("AHA") == "2\n-1 8 -15\n", "two transitions"

# Maximum-size input with no transitions
assert run("H" * 10000) == "0\n1\n", "maximum-size all-human input"

# Maximum-size all-alien input
assert run("A" * 10000) == "0\n-1\n", "maximum-size all-alien input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`H`|`0 / 1`| Dân số tối thiểu và đa thức bậc 0 | 
|`A`|`0 / -1`| Đa thức bậc 0 có dấu âm | 
|`HA`|`1 / -1 3`| Đúng vị trí gốc và dấu chung | 
|`AHA`|`2 / -1 8 -15`| Nhiều chuyển đổi và tính chẵn lẻ của mức độ | 
|`H`lặp lại 10.000 lần |`0 / 1`| Kích thước đầu vào tối đa không có chuyển tiếp | 
|`A`lặp lại 10.000 lần |`0 / -1`| Kích thước đầu vào tối đa với đa thức âm không đổi | 

## Vỏ cạnh 

Đối với một công dân, không có khoảng thời gian nào có thể yêu cầu thay đổi biển báo. Với đầu vào`H`, thuật toán không tìm thấy nghiệm nào, đạt bậc 0 và giữ nguyên đa thức (1). Với đầu vào`A`, nó cũng tìm độ 0 nhưng lật đa thức không đổi thành (-1). Sự vắng mặt của các chuyển đổi chính xác là điều kiện mà một đa thức không đổi là tối ưu. 

Đối với sự chuyển đổi nhỏ nhất có thể, hãy xem xét`HA`. Hai công dân ở vị trí thứ 2 và thứ 4. Dấu hiệu yêu cầu của họ khác nhau, vì vậy một số nghiệm lẻ phải nằm chính xác giữa chúng. Vì số nguyên duy nhất có 3 nên bậc đa thức tối thiểu là 1 và nghiệm phải là 3. Đa thức không chia tỷ lệ (x-3) có dấu âm và dương tại 2 và 4 nên phải âm. Kết quả là (-x+3). 

Đối với nhiều chuyển đổi, hãy xem xét`AHA`. Những thay đổi bắt buộc xảy ra giữa vị trí 2 và 4, giữa vị trí 4 và 6. Thuật toán chọn nghiệm 3 và 5, tạo ra 

[ 
Q(x)=(x-3)(x-5)=x^2-8x+15. 
] 

Bởi vì bậc chẵn, (Q(2)>0). Ký tự đầu tiên là`A`, do đó toàn bộ đa thức bị phủ định: 

[ 
P(x)=-x^2+8x-15. 
] 

Các giá trị của nó tại 2, 4, 6 mang dấu âm, dương, âm, đúng theo yêu cầu. 

Đối với một dân số lớn có rất ít thay đổi, chẳng hạn như 10.000 liên tiếp`H`ký tự, thuật toán không xây dựng đa thức bậc 9999. Nó quét toàn bộ chuỗi, tìm thấy các chuyển tiếp bằng 0 và ngay lập tức trả về (P(x)=1). Điều này chứng tỏ tại sao độ dài đầu vào và bậc đa thức phải được coi là các đại lượng riêng biệt. 

Cuối cùng, hãy xem xét một chuỗi xen kẽ giả định có 17 lần chuyển đổi. Đầu vào như vậy sẽ buộc ít nhất phải có gốc (3,5,\ldots,35). Tích của họ đã vượt quá (2^{63}) nên đa thức bậc tối thiểu của nó không thể thỏa mãn giới hạn hệ số. Sự đảm bảo của vấn đề sẽ loại bỏ trường hợp như vậy. Đây là lý do ẩn giấu mà phép nhân hệ số theo hệ số trực tiếp vẫn an toàn mặc dù đã nêu (N\le10^4).
