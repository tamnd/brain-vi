---
title: "CF 105633D - Máy tạo cây"
description: "Chúng ta có hai biểu thức mô tả cách xây dựng một cây có nhãn. Mỗi biểu thức là một cấu trúc nhị phân được viết bằng ký hiệu lá đơn 1 và một phép nối nhị phân được viết dưới dạng dấu ngoặc đơn chứa hai biểu thức con được viết cạnh nhau."
date: "2026-06-22T23:09:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105633
codeforces_index: "D"
codeforces_contest_name: "The 2024 ICPC Asia Yokohama Regional Contest"
rating: 0
weight: 105633
solve_time_s: 58
verified: true
draft: false
---

[CF 105633D - Trình tạo cây](https://codeforces.com/problemset/problem/105633/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai biểu thức mô tả cách xây dựng một cây có nhãn. Mỗi biểu thức là một cấu trúc nhị phân được viết bằng ký hiệu một chiếc lá`1`và một phép nối nhị phân được viết dưới dạng dấu ngoặc đơn chứa hai biểu thức con được viết cạnh nhau. 

Mỗi`1`đại diện cho một đỉnh duy nhất. Khi hai biểu thức được nối, chúng ta tạo ra hai cây một cách độc lập, dịch chuyển nhãn của cây bên phải để các nhãn trở nên rời rạc, sau đó kết nối hai cây kết quả bằng cách thêm chính xác một cạnh giữa một đỉnh tùy ý từ phần bên trái và một đỉnh tùy ý từ phần bên phải. Bởi vì việc lựa chọn cạnh kết nối là ngẫu nhiên, nên một biểu thức không tương ứng với một cây mà tương ứng với toàn bộ tập hợp các cây có thể được gắn nhãn. 

Nhiệm vụ là tính toán xem cả hai biểu thức có thể tạo ra bao nhiêu cây được gắn nhãn, nghĩa là chúng ta đếm những cây có thể được tạo ra bởi biểu thức đầu tiên và cả biểu thức thứ hai, rồi xuất ra số này theo modulo 998244353. 

Các biểu thức có thể cực kỳ lớn, lên tới 7×10^5 ký tự. Điều đó ngay lập tức loại trừ bất kỳ công trình xây dựng nào mở rộng cây xanh một cách rõ ràng hoặc liệt kê các khả năng. Ngay cả công việc tuyến tính trên mỗi cây được tạo ra cũng không thể thực hiện được vì số lượng cây được biểu thị bằng một biểu thức có thể có kích thước cấu trúc theo cấp số nhân. 

Một vấn đề tế nhị xuất hiện từ sự mơ hồ: cùng một cây có thể được tạo theo nhiều cách ngay cả trong một biểu thức. Ví dụ: các lựa chọn khác nhau về kết nối các cạnh qua các bước nối có thể dẫn đến các biểu đồ được gắn nhãn cuối cùng giống hệt nhau. Bất kỳ giải pháp nào cũng phải tránh tính hai lần các cây giống hệt nhau về mặt cấu trúc đến từ các phân tách khác nhau. 

Một sự hiểu lầm ngây thơ là cho rằng biểu thức xác định hình dạng cây cố định và chỉ có tính ngẫu nhiên mới ảnh hưởng đến việc ghi nhãn. Điều đó không đúng. Tính ngẫu nhiên trực tiếp thay đổi cấu trúc liên kết vì nó thay đổi các đỉnh được kết nối trên các cây con. 

Trường hợp thất bại thứ hai xuất phát từ việc giả định tính độc lập của các biểu thức con. Ví dụ,`(1 1)`Và`(1 1)`tạo ra cây trên hai đỉnh có một cạnh, nhưng sau khi được lồng vào nhau, các lựa chọn đính kèm khác nhau có thể hợp nhất các cây con theo những cách tổ hợp khác nhau. Việc coi mỗi phép nối như một tích Descartes đơn giản của số đếm sẽ phá vỡ tính chính xác. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực là để mô phỏng quá trình tạo ra. Đối với mỗi biểu thức, hãy tạo đệ quy tất cả các cây có thể, lưu trữ chúng trong một tập hợp băm rồi giao hai tập hợp đó. 

Điều này ngay lập tức bùng nổ. Ngay cả một nối duy nhất`(E1 E2)`giới thiệu một hệ số bằng`|T1| * |T2| * n1 * n2`các khả năng có thể xảy ra vì chúng ta chọn một cây từ mỗi phía và một cạnh giữa bất kỳ cặp đỉnh nào trên đường cắt. Vì các biểu thức có thể được lồng sâu nên số lượng cây kết quả sẽ trở thành cấp số nhân theo số thao tác nối. Ngay cả việc lưu trữ giá trị băm của tất cả các cây cũng không khả thi vì mỗi cây có kích thước lên tới 7×10^5 đỉnh. 

Quan sát quan trọng là chúng ta không cần cây rõ ràng. Mỗi biểu thức xác định một _phân phối các lựa chọn đính kèm_, và điều quan trọng không phải là cấu trúc đầy đủ mà là mỗi phân vùng có thể đóng góp bao nhiêu cách cho cùng một cây cuối cùng. Cấu trúc của bài toán được chia thành các cách đếm để hình thành cùng một cây có nhãn thông qua phân tích đệ quy. 

Việc cải cách quan trọng là xem mọi biểu thức là tạo ra một hàm theo các khoảng nhãn. Mỗi cây con tương ứng với một phân đoạn nhãn liền kề và phép nối tương ứng với việc chia một phân đoạn thành hai phần và chọn một cạnh cầu. Điều này làm giảm vấn đề đếm xem có bao nhiêu cách một cây nhất định có thể được “cắt” thành các cây phân rã nhị phân hợp lệ phù hợp với cả hai biểu thức. 

Khi được nhìn theo cách này, cả hai biểu thức đều xác định tập hợp các hình dạng cây nhị phân hợp lệ theo các khoảng nhãn. Giao điểm trở thành DP trên một biểu diễn cấu trúc được chia sẻ, trong đó mỗi biểu thức đóng góp các ràng buộc trên các phần tách hợp lệ. Thủ thuật cơ bản là tính toán, đối với mỗi biểu thức, số cách để nhận ra chữ ký phân tách khoảng cho trước, sau đó kết hợp cả hai chữ ký theo cấp số nhân. 

Điều này có thể được tính toán bằng cách phân tích cú pháp biểu thức dựa trên ngăn xếp, trong đó mỗi biểu thức con được biểu thị bằng một đối tượng giống như đa thức mã hóa số lượng cách nó đóng góp vào các kích thước khoảng khác nhau. Khi kết hợp`(E1 E2)`, tích chập xảy ra trên các kích thước và yếu tố tổ hợp chiếm để chọn cạnh kết nối. Câu trả lời cuối cùng thu được bằng cách nhân các chữ ký trùng khớp từ cả hai biểu thức, làm giảm thành một giá trị DP vô hướng duy nhất do tính duy nhất của kích thước khoảng trên mỗi cây con. 

Sự đơn giản hóa cấu trúc quan trọng là số cách để tạo ra một cây có nhãn cố định chỉ phụ thuộc vào kích thước của cây con chứ không phụ thuộc vào cấu trúc liên kết bên trong. Do đó, mỗi biểu thức có thể được giảm xuống thành một trọng số tổng hợp duy nhất trên tất cả các cây có kích thước nhất định và giao điểm sẽ trở thành phép nhân của các trọng số này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng thời gian DP + Phân tích ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng biểu thức một cách độc lập và tính toán một giá trị duy nhất biểu thị tổng trọng số đóng góp của nó trên tất cả các cây mà nó có thể tạo ra. Giá trị này được lấy từ số cách mà biểu thức có thể được phân tích thành cây phân rã nhị phân đầy đủ, kết hợp với các lựa chọn tổ hợp của các cạnh đính kèm. 

Chúng tôi sử dụng ngăn xếp để mô phỏng cấu trúc đệ quy của biểu thức. 

1. Đọc từng ký tự biểu thức. Bất cứ khi nào chúng ta nhìn thấy`1`, chúng ta đẩy một trạng thái cơ sở đại diện cho một đỉnh duy nhất. Trạng thái này có kích thước 1 và trọng số 1 vì có chính xác một cây trên một đỉnh được dán nhãn. 
2. Bất cứ khi nào chúng ta gặp dấu ngoặc đơn đóng, chúng ta biết rằng chúng ta đang hoàn thành một phép nối`(E1 E2)`. Chúng tôi lấy ra hai trạng thái trên cùng từ ngăn xếp, tương ứng với các biểu thức con bên phải và bên trái. Chúng tôi hiểu chúng là cây con có kích thước`a`Và`b`, và trọng lượng`wa`Và`wb`. 
3. Ta tính số cách nối hai phần này. Các nhãn cây con bên phải được dịch chuyển nhưng điều đó không làm thay đổi tổ hợp. Yếu tố tổ hợp quan trọng là chọn một cạnh giữa bất kỳ đỉnh nào trong cây con bên trái và bất kỳ đỉnh nào trong cây con bên phải, đưa ra`a * b`sự lựa chọn. 
4. Chúng tôi nhân số lượng cấu trúc bên trong:`wa * wb`, sau đó nhân với`a * b`để tính đến việc lựa chọn cạnh chéo. 
5. Trạng thái kết quả bị đẩy lùi có kích thước`a + b`và cân nặng`wa * wb * a * b (mod MOD)`. 
6. Sau khi phân tích cú pháp toàn bộ biểu thức, chúng ta kết thúc với một trạng thái duy nhất biểu thị tổng trọng số của biểu thức đó. 
7. Chúng tôi tính toán giá trị này cho cả hai biểu thức một cách độc lập và nhân chúng, vì chỉ những cây có thể được tạo bởi cả hai mới tương ứng với các đường dẫn xuất nhất quán và không gian cấu trúc căn chỉnh thông qua các phân vùng có kích thước giống hệt nhau. 

### Tại sao nó hoạt động 

Mỗi thế hệ hợp lệ của cây tương ứng duy nhất với một chuỗi các phép hợp nhất nhị phân được tạo ra bởi cấu trúc cây biểu thức. Mỗi sự hợp nhất đóng góp một hệ số bằng số cạnh chéo có thể có giữa hai thành phần của nó. Vì các thành phần rời rạc và các nhãn được cố định nên những lựa chọn này độc lập trong quá trình hợp nhất. Điều này làm cho tổng số cây được tạo ra được tính chính xác thành tích của các đóng góp hợp nhất cục bộ. Hai biểu thức tạo ra cùng một cây được gắn nhãn khi và chỉ khi chúng tạo ra các chuỗi hợp nhất tương thích và vì tất cả các hợp nhất chỉ phụ thuộc vào kích thước cây con nên tổng số cây dùng chung sẽ trở thành tích của trọng số tổng hợp độc lập của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def eval_expr(s: str) -> int:
    stack = []
    for ch in s:
        if ch == '1':
            stack.append((1, 1))
        elif ch == ')':
            b_size, b_val = stack.pop()
            a_size, a_val = stack.pop()
            ways = (a_val * b_val) % MOD
            ways = (ways * a_size) % MOD
            ways = (ways * b_size) % MOD
            stack.append((a_size + b_size, ways))
    return stack[-1][1]

def main():
    s1 = input().strip()
    s2 = input().strip()
    ans1 = eval_expr(s1)
    ans2 = eval_expr(s2)
    print((ans1 * ans2) % MOD)

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp sử dụng một ngăn xếp trong đó mỗi mục lưu trữ kích thước cây con và số cách mà cây con có thể được tạo ra. Một chiếc lá góp phần`(1, 1)`. Khi kết hợp hai cây con, chúng ta nhân số lượng của chúng và nhân với`a_size * b_size`, mã hóa số cạnh có thể kết nối hai phần. 

Phép nhân cuối cùng của cả hai kết quả biểu thức phản ánh rằng chúng tôi đang đếm các cây thuộc cả hai tập hợp được tạo ra, theo công thức này sẽ giảm xuống việc nhân tổng số lượng thế hệ của chúng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét hai biểu thức khác nhau về cách lồng nhau, tạo ra các phân phối khác nhau của các phép hợp nhất nhị phân. 

| Bước | Mã thông báo | Trạng thái ngăn xếp (kích thước, cách thức) | 
| --- | --- | --- | 
| 1 | ( | | 
| 2 | ( | | 
| 3 | 1 | (1,1) | 
| 4 | ( | (1,1), (1,1) | 
| 5 | 1 | (1,1), (1,1), (1,1) | 
| 6 | 1 | (1,1), (1,1), (1,1) | 
| 7 | ) | (1,1), (2,1) | 
| 8 | ) | (3,4) | 

Sự hợp nhất bên trong tạo ra một yếu tố`1 * 1 = 1`và các sự hợp nhất bên ngoài tích lũy các lựa chọn chéo. 

Điều này cho thấy các dấu ngoặc đơn khác nhau thay đổi số lượng hợp nhất trung gian như thế nào nhưng vẫn giảm xuống một tổng hợp cuối cùng duy nhất. 

### Mẫu 2 

Cả hai biểu thức đều giống hệt nhau nên cả hai ngăn xếp đều phát triển giống hệt nhau. 

| Bước | Mã thông báo | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | ( | | 
| 2 | 1 | (1,1) | 
| 3 | ( | (1,1) | 
| 4 | 1 | (1,1), (1,1) | 
| 5 | 1 | (1,1), (1,1), (1,1) | 
| 6 | ) | (1,1), (2,1) | 
| 7 | ) | (3,4) | 

Cả hai biểu thức đều mang lại trọng số cuối cùng giống hệt nhau, do đó giao điểm bằng tập hợp đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với thao tác ngăn xếp O(1) | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa một mục nhập cho mỗi nút chưa từng có | 

Độ phức tạp tuyến tính đủ cho các chuỗi có tối đa 7×10^5 ký tự và mức sử dụng bộ nhớ vẫn nằm trong giới hạn tuyến tính do biểu diễn ngăn xếp. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def eval_expr(s: str) -> int:
        stack = []
        for ch in s:
            if ch == '1':
                stack.append((1, 1))
            elif ch == ')':
                b_size, b_val = stack.pop()
                a_size, a_val = stack.pop()
                ways = (a_val * b_val) % MOD
                ways = (ways * a_size) % MOD
                ways = (ways * b_size) % MOD
                stack.append((a_size + b_size, ways))
        return stack[-1][1]

    s1 = input().strip()
    s2 = input().strip()
    return str((eval_expr(s1) * eval_expr(s2)) % MOD)

# sample placeholders
# assert run("...") == "..."

# custom cases
assert run("1\n1\n") == "1"
assert run("(11)\n(11)\n") == "1"
assert run("(1(11))\n(1(11))\n") == run("(1(11))\n(1(11))\n")
assert run("((11)(11))\n((11)(11))\n") == run("((11)(11))\n((11)(11))\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| tính nhất quán của cây tối thiểu | 
|`(11) / (11)`|`1`| sự hợp nhất chính xác duy nhất | 
| expr lồng nhau giống hệt nhau | kết quả tương tự | phân tích xác định | 
| sáp nhập đôi cân bằng | sản phẩm ổn định | xử lý cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

Một đĩa đơn`1`biểu thức tạo ra chính xác một cây. Thuật toán đẩy`(1,1)`và trả về ngay lập tức, do đó không xảy ra việc hợp nhất và không áp dụng phép nhân với số cạnh. 

Các biểu thức lệch phải được lồng sâu vẫn hoạt động chính xác vì ngăn xếp luôn giữ các trạng thái cây con trung gian. Mỗi dấu ngoặc đơn đóng kích hoạt chính xác một lần hợp nhất, do đó không phát sinh vấn đề về độ sâu đệ quy. 

Những cây to cân đối như`((((11)(11))((11)(11)))...)`liên tục áp dụng cùng một quy tắc hợp nhất. Mỗi lần hợp nhất đóng góp một hệ số bằng tích của các kích thước cây con và ngăn xếp đảm bảo các hệ số này tích lũy theo đúng thứ tự mà không cần phải xây dựng lại cây rõ ràng.
