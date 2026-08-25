---
title: "CF 102202D - A Cộng Bằng B"
description: "Chúng ta bắt đầu với hai số nguyên dương A và B. Cách duy nhất để thay đổi chúng là cộng một trong các giá trị hiện tại vào một trong hai biến. Do đó, một phép toán có thể nhân đôi một biến hoặc thay thế một biến bằng tổng của cả hai."
date: "2026-08-24T16:10:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "D"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 2420
verified: false
draft: false
---

[CF 102202D - A Cộng Bằng B](https://codeforces.com/problemset/problem/102202/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai số nguyên dương,`A`Và`B`. Cách duy nhất để thay đổi chúng là thêm một trong các giá trị hiện tại vào một trong hai biến. Do đó, một phép toán có thể nhân đôi một biến hoặc thay thế một biến bằng tổng của cả hai. Mục tiêu là xuất ra bất kỳ chuỗi nào gồm tối đa 5000 thao tác như vậy làm cho hai giá trị bằng nhau. Đầu vào chứa các giá trị ban đầu của`A`Và`B`, mỗi cái nhiều nhất\(10^{18}\). Tuyên bố chính thức của cuộc thi có chính xác cách xây dựng và giới hạn này. citturn3search1 

Sự ràng buộc của\(10^{18}\)ngay lập tức loại trừ bất kỳ phương pháp nào có số lượng thao tác phụ thuộc tuyến tính vào các giá trị. Một công trình cần\(A+B\),\(|A-B|\), hoặc thậm chí các bước \(\max(A,B)\) có thể yêu cầu khoảng\(10^{18}\)hoạt động. Giới hạn hoạt động 5000 là ràng buộc thuật toán thực sự, vì vậy chúng ta cần một cấu trúc logarit hoặc nhỏ tương tự. Vì đầu vào chỉ chứa hai số nguyên nên không cần cấu trúc dữ liệu phức tạp và giới hạn bộ nhớ đủ lớn để việc lưu trữ vài nghìn chuỗi thao tác là chuyện nhỏ. 

Trường hợp cạnh đầu tiên là sự bình đẳng ngay từ đầu. Đối với đầu vào`5 5`, đầu ra đúng chỉ đơn giản là`0`, vì không cần thao tác. Việc triển khai bất cẩn luôn đi vào vòng rút gọn có thể thực hiện các hoạt động không cần thiết và nghiêm trọng hơn là có thể làm mất đi câu trả lời đã hợp lệ. 

Trường hợp cạnh thứ hai là một giá trị chẵn. Đối với đầu vào`4 6`, về mặt khái niệm chúng ta có thể giảm cặp này thành`(1, 3)`bằng cách giảm một nửa cả hai giá trị, nhưng các phép toán được phép không chứa phép chia. Việc xây dựng phải thực hiện những sự phân chia đó một cách gián tiếp. Xuất ra`B+=B`tương ứng với điều trị`A`giảm một nửa sau khi loại bỏ thừa số chung của hai, trong khi`A+=A`đóng vai trò đối xứng`B`. Viết trực tiếp`A//=2`không thực hiện phép tính tương ứng sẽ tạo ra một phép tính hữu ích về mặt toán học nhưng lại là một câu trả lời không hợp lệ. 

Trường hợp cạnh thứ ba là một cặp giá trị lẻ. Đối với đầu vào`3 5`, cộng giá trị nhỏ hơn vào giá trị lớn hơn`(3, 8)`. Giá trị mới lớn hơn là chẵn nên nó có thể giảm đi một nửa trong biểu diễn chuẩn hóa. Thay vào đó, nếu chúng ta liên tục cộng giá trị nhỏ hơn mà không sử dụng tính chẵn lẻ, thì các giá trị có thể tăng lên thay vì đạt đến mức bằng nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tìm kiếm thông qua các chuỗi hoạt động có thể. Từ mỗi trạng thái có bốn lựa chọn, vì vậy sau\(k\)các bước có thể có nhiều như\(4^k\)trình tự cần xem xét. Việc tìm kiếm tới độ sâu cho phép là 5000 là hoàn toàn không thể, vì số lượng chuỗi theo thứ tự\(4^{5000}\). Ngay cả các trạng thái ghi nhớ cũng không đưa ra giới hạn hữu ích vì giá trị tăng nhanh và có quá nhiều trạng thái có thể tiếp cận. 

Một nỗ lực mang tính xây dựng tự nhiên hơn là sử dụng phép cộng theo tinh thần tương tự như thuật toán Euclide. Vấn đề là thuật toán Euclide muốn phép trừ, trong khi mọi phép toán khả dụng chỉ là phép cộng. Ví dụ, bắt đầu từ`(1, 1000000000000000000)`, việc cộng liên tục giá trị nhỏ hơn vào giá trị lớn hơn sẽ cần một số lượng lớn các phép cộng nếu chúng ta cố gắng mô phỏng trực tiếp phép trừ. 

Quan sát quan trọng là việc nhân cả hai số với cùng một thừa số dương không ảnh hưởng đến việc chúng có bằng nhau hay không. Chúng ta có thể tự do suy luận về một cặp chuẩn hóa thu được bằng cách chia các thừa số chung của hai. Điều này cho phép chúng tôi diễn giải lại các hoạt động nhân đôi thành các hoạt động giảm một nửa ảo. 

Giả sử cặp chuẩn hóa là`(A, B)`Và`A`là chẵn. Nếu chúng ta thực hiện thao tác thực sự`B+=B`, cặp vật lý trở thành`(A, 2B)`. Nếu chúng ta chia cả hai giá trị cho hai một cách khái niệm thì điều này tương đương với`(A/2, B)`. Do đó, một phép toán hợp pháp mang lại cho chúng ta trạng thái chuẩn hóa tương tự như giảm một nửa`A`. Tương tự,`A+=A`có thể được coi là giảm một nửa`B`trong biểu diễn chuẩn hóa. 

Khi cả hai giá trị chuẩn hóa đều là số lẻ, giả sử`A < B`. Chúng tôi biểu diễn`B+=A`, cho`(A, A+B)`. Vì cả hai toán hạng đều là số lẻ,`A+B`là số chẵn, do đó phép chuẩn hóa tiếp theo có thể chia giá trị lớn hơn đó cho hai. Sự chuyển đổi khái niệm là\[
(A,B)\rightarrow \left(A,\frac{A+B}{2}\right).
\]Đây là mức giảm quan trọng. Nếu như`B-A=C`, thì sau quá trình chuyển đổi này, sự khác biệt mới là\[
\frac{A+B}{2}-A=\frac{B-A}{2}=\frac C2.
\]Vì vậy, khi cả hai giá trị đều là số lẻ, một phép cộng theo sau là sự chuẩn hóa sẽ làm giảm gần một nửa sự khác biệt. Đây là ý tưởng tương tự về tính chẵn lẻ đằng sau thuật toán Euclide nhị phân. 

Việc xây dựng kết quả liên tục loại bỏ các thừa số của 2, sau đó cộng giá trị lẻ nhỏ hơn vào giá trị lẻ lớn hơn. Các giá trị chuẩn hóa giảm dần cho đến khi cả hai trở thành một, tại thời điểm đó các giá trị thực tế bằng nhau vì tất cả các phép biến đổi đều bảo toàn quan hệ đẳng thức. Đối số đếm thao tác chi tiết đưa ra giới hạn dưới 5000, với phân tích chặt chẽ hơn đưa ra 3969 thao tác cho các giá trị lên tới\(10^{18}\). citturn4view0 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---:|---:|---| 
| Lực lượng vũ phu | \(O(4^{5000})\) không gian chuỗi | \(O(5000)\) mỗi chuỗi | Quá chậm | 
| Tối ưu | \(O(\log^2 10^{18})\) | \(O(\log^2 10^{18})\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ`a`Và`b`như các giá trị chuẩn hóa được sử dụng để suy luận. Chúng không đại diện cho các giá trị theo nghĩa đen được giám khảo lưu trữ sau mỗi thao tác. Thay vào đó, chúng đại diện cho một cặp tương đương sau khi loại bỏ các thừa số chung của hai. 

2. Trong khi`a`chẵn, nối thêm`B+=B`để trả lời và thay thế`a`qua`a // 2`. Hoạt động thực sự tăng gấp đôi`B`và sau khi chia cả hai giá trị cho hai về mặt khái niệm, trạng thái chuẩn hóa hoàn toàn giống như giảm một nửa`a`. 

3. Trong khi`b`chẵn, nối thêm`A+=A`và thay thế`b`qua`b // 2`. Đây là phép biến đổi đối xứng. Hoạt động thực sự tăng gấp đôi`A`, tương đương với việc giảm một nửa`B`sau khi loại bỏ thừa số chung của 2. 

4. Nếu`a == b`, dừng lại. Các giá trị được chuẩn hóa là bằng nhau, do đó các giá trị thực tế được biểu thị bằng chúng cũng bằng nhau. 

5. Nếu cả hai giá trị chuẩn hóa bây giờ đều là số lẻ và`a < b`, nối thêm`B+=A`và thay thế`b`qua`a + b`. Tổng của hai số lẻ là số chẵn nên lần lặp tiếp theo có thể giảm đi một nửa`b`. 

6. Nếu cả hai giá trị chuẩn hóa đều là số lẻ và`b < a`, nối thêm`A+=B`và thay thế`a`qua`a + b`. Một lần nữa, giá trị lớn hơn được cập nhật là chẵn và sẽ giảm một nửa khi bắt đầu lần lặp tiếp theo. 

Lý do khiến quá trình tiến bộ dễ dàng nhận thấy nhất từ ​​​​sự khác biệt. Cho rằng`a < b`và cả hai đều kỳ quặc. Sau khi cộng và giảm một nửa tiếp theo, sự khác biệt sẽ trở thành`(b-a)/2`. Do đó, khoảng cách giữa hai giá trị chuẩn hóa sẽ giảm đi hai lần bất cứ khi nào cần bước bổ sung. Các bước loại bỏ chẵn lẻ sẽ tự giảm số lượng. 

### Tại sao nó hoạt động 

Bất biến là cặp chuẩn hóa`(a, b)`đại diện cho cặp thực tế hiện tại khi nhân cả hai giá trị với cùng lũy ​​thừa dương của hai. Tỷ lệ chung như vậy không thể thay đổi liệu hai giá trị có bằng nhau hay không. 

Khi`a`là chẵn,`B+=B`thay đổi cặp thực tế từ`(A,B)`ĐẾN`(A,2B)`. Chia cả hai cho hai được`(A/2,B)`, vì vậy thay thế chuẩn hóa`a`qua`a/2`là hợp pháp. Lập luận tương tự được áp dụng khi`b`là chẵn. 

Khi cả hai giá trị chuẩn hóa đều là số lẻ và không bằng nhau, việc cộng giá trị nhỏ hơn với giá trị lớn hơn là một phép toán hợp pháp. Giá trị lớn hơn thu được là chẵn và phép chuẩn hóa sau sẽ giảm một nửa giá trị đó. Nếu giá trị nhỏ hơn là`a`, cặp này thay đổi về mặt khái niệm từ`(a,b)`ĐẾN`(a,(a+b)/2)`. Sự khác biệt mới là`(b-a)/2`, do đó quá trình không thể giữ nguyên hiệu số dương mãi mãi. Cuối cùng, các giá trị chuẩn hóa đạt đến sự bằng nhau và bất biến sau đó cho chúng ta biết rằng các giá trị thực tế cũng bằng nhau. 

Số lượng hoạt động cũng đủ nhỏ cho giới hạn 5000. Một giới hạn hữu ích nhóm quy trình thành các giai đoạn trong đó tích của các giá trị chuẩn hóa được giảm ít nhất theo hệ số hai. Mỗi giai đoạn như vậy cần nhiều nhất\(2\lfloor\log_2 B\rfloor+1\)hoạt động và vì sản phẩm ban đầu thấp hơn\(10^{36}\), tính tổng các giới hạn này sẽ có ít hơn 7200 phép toán. Phân tích pha sắc nét hơn sẽ đưa ra giới hạn yêu cầu là 3969, ở mức dưới 5000 một cách thoải mái. citeturn6view0turn4view0 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    ans = []

    while a != b:
        while a % 2 == 0:
            ans.append("B+=B")
            a //= 2

        while b % 2 == 0:
            ans.append("A+=A")
            b //= 2

        if a == b:
            break

        if a < b:
            ans.append("B+=A")
            b += a
        else:
            ans.append("A+=B")
            a += b

    print(len(ans))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần vì bài toán có một cặp giá trị. các`ans`mảng lưu trữ các hoạt động pháp lý chính xác phải được in. 

đầu tiên`while`vòng lặp xử lý mọi thừa số của hai trong`a`. Hoạt động được thêm vào câu trả lời là`B+=B`, không`A+=A`, bởi vì việc nhân đôi giá trị thực tế còn lại sẽ khiến cho việc giảm một nửa`A`hợp lệ sau khi chuẩn hóa chung. Sự đảo ngược này là chi tiết triển khai dễ mắc sai lầm nhất. 

Vòng lặp thứ hai thực hiện phép toán đối xứng cho`b`. Vì giá trị ban đầu nhiều nhất là\(10^{18}\), số nguyên của Python là quá đủ và không có vấn đề tràn. 

Sau cả hai vòng lặp, nếu các giá trị chuẩn hóa bằng nhau thì quá trình xây dựng đã hoàn tất. Nếu không thì cả hai đều kỳ quặc. Chính xác là một cái lớn hơn, vì vậy chúng ta cộng cái nhỏ hơn vào cái lớn hơn. Giá trị mới lớn hơn là chẵn, đảm bảo công việc hữu ích trong lần lặp tiếp theo. 

Mã không mô phỏng rõ ràng các giá trị thực tế sau mỗi thao tác. Làm như vậy là không cần thiết và có thể làm cho lý do trở nên khó hiểu. Các giá trị được chuẩn hóa là đủ vì mỗi bước giảm một nửa thể hiện việc loại bỏ hệ số chung của hai khỏi cặp thực tế. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên, đầu vào là`2 3`. 

| Bước | Chuẩn hóa`a`| Chuẩn hóa`b`| Hoạt động | 
|---:|---:|---:|---| 
| 0 | 2 | 3 | bắt đầu | 
| 1 | 1 | 3 |`B+=B`| 
| 2 | 1 | 4 |`B+=A`| 
| 3 | 1 | 2 |`A+=A`| 
| 4 | 1 | 1 |`A+=A`| 

Trình tự thực tế tương ứng là```text
B+=B
B+=A
A+=A
A+=A
```Bắt đầu từ`(2,3)`, các giá trị thực tế trở thành`(2,6)`,`(2,8)`,`(4,8)`, và cuối cùng`(8,8)`. Dấu vết chuẩn hóa phân chia lũy thừa chung của hai và đạt đến`(1,1)`. 

Đối với ví dụ thứ hai, hãy xem xét`4 6`. 

| Bước | Chuẩn hóa`a`| Chuẩn hóa`b`| Hoạt động | 
|---:|---:|---:|---| 
| 0 | 4 | 6 | bắt đầu | 
| 1 | 2 | 6 |`B+=B`| 
| 2 | 1 | 6 |`B+=B`| 
| 3 | 1 | 3 |`A+=A`| 
| 4 | 1 | 4 |`B+=A`| 
| 5 | 1 | 2 |`A+=A`| 
| 6 | 1 | 1 |`A+=A`| 

Các giá trị thực tế theo sau`(4,6)`,`(4,12)`,`(4,24)`,`(8,24)`,`(8,32)`,`(16,32)`,`(32,32)`. Các giá trị chuẩn hóa được phép thu nhỏ ngay cả khi các giá trị thực tế không bao giờ giảm, bởi vì mỗi lần chuẩn hóa sẽ loại bỏ một hệ số chung khỏi cả hai giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---:|---| 
| Thời gian | \(O(\log^2 V)\) | Có nhiều phép giảm chẵn lẻ theo logarit giữa các giai đoạn và mỗi giai đoạn thực hiện tối đa nhiều phép cộng và giảm theo logarit. | 
| Không gian | \(O(\log^2 V)\) | Danh sách thao tác chứa ít hơn 5000 chuỗi. | 

Ở đây \(V=\max(A,B)\le10^{18}\). Công trình có hoạt động đã được chứng minh là giới hạn dưới 5000, do đó nó đáp ứng giới hạn đầu ra đặc biệt. Python chỉ thực hiện vài nghìn phép tính số nguyên và lưu trữ vài nghìn chuỗi ngắn, dễ dàng nằm trong giới hạn 1 giây và 1024 MB. citturn6view0 

## Trường hợp thử nghiệm 

Bởi vì đây là một bài toán xây dựng thẩm phán đặc biệt nên không có một chuỗi đầu ra bắt buộc nào. Bộ khai thác kiểm tra bên dưới kiểm tra xem trình tự được tạo ra chỉ chứa các thao tác hợp pháp, có tối đa 5000 thao tác và thực sự làm cho hai giá trị ban đầu bằng nhau.```python
# helper: run solution on input string, return output string
import sys
 < b:
                ans.append("B+=A")
                b += a
            else:
                ans.append("A+=B")
                a += b

        print(len(ans))
        print("\n".join(ans))
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solution(inp)

def validate(inp: str, out: str):
    a, b = map(int, inp.split())
    lines = out.strip().splitlines()

    assert lines
    n = int(lines[0])
    assert 0 <= n <= 5000
    assert len(lines) == n + 1

    allowed = {"A+=A", "A+=B", "B+=A", "B+=B"}

    for op in lines[1:]:
        assert op in allowed

        if op == "A+=A":
            a += a
        elif op == "A+=B":
            a += b
        elif op == "B+=A":
            b += a
        else:
            b += b

    assert a == b

# Provided sample
out = run("2 3")
validate("2 3", out)

# Minimum-size input
out = run("1 1")
validate("1 1", out)
assert out.strip() == "0"

# Even values with several factors of two
out = run("4 6")
validate("4 6", out)

# Boundary values
out = run("1 1000000000000000000")
validate("1 1000000000000000000", out)

# Maximum-size equal values
out = run("1000000000000000000 1000000000000000000")
validate("1000000000000000000 1000000000000000000", out)
assert out.strip() == "0"

# Close odd values, useful for catching parity mistakes
out = run("999999999999999999 1000000000000000000")
validate("999999999999999999 1000000000000000000", out)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`2 3`| Bất kỳ chuỗi hợp lệ nào, bao gồm 4 thao tác của mẫu | Cung cấp mẫu và các chuyển đổi lẻ/chẵn cơ bản | 
|`1 1`|`0`| Trường hợp ranh giới đã bằng nhau | 
|`4 6`| Bất kỳ chuỗi hợp lệ nào | Giảm một nửa lặp đi lặp lại của các giá trị chuẩn hóa | 
|`1 1000000000000000000`| Bất kỳ chuỗi hợp lệ nào có tối đa 5000 thao tác | Giá trị cực kỳ mất cân bằng | 
|`1000000000000000000 1000000000000000000`|`0`| Giá trị đầu vào tối đa đã bằng nhau | 
|`999999999999999999 1000000000000000000`| Bất kỳ chuỗi hợp lệ nào | Các giá trị liền kề và chuyển tiếp nhạy cảm với chẵn lẻ | 

## Vỏ cạnh 

cho`5 5`, thuật toán đi vào điều kiện vòng lặp bên ngoài`a != b`, thấy rằng nó đã sai và ngay lập tức in ra các thao tác bằng 0. Đây là cách xây dựng hợp lý duy nhất vì điều kiện mục tiêu đã được đảm bảo. 

Vì`4 6`, thuật toán chuẩn hóa trước tiên sẽ loại bỏ các thừa số của hai khỏi`a`. đầu tiên`B+=B`đại diện cho`4 -> 2`, và thứ hai đại diện cho`2 -> 1`. Sau đó`A+=A`đại diện cho`6 -> 3`. Trạng thái chuẩn hóa bây giờ là`(1,3)`. hoạt động`B+=A`thay đổi nó về mặt khái niệm thành`(1,4)`, sau đó hai`A+=A`hoạt động đại diện`4 -> 2 -> 1`. Trạng thái bình thường hóa đạt đến`(1,1)`, và giá trị thực tế đã đạt tới`(32,32)`. 

Vì`1 1000000000000000000`, giá trị lớn chứa nhiều thừa số của hai. Thuật toán loại bỏ các yếu tố đó bằng cách sử dụng`A+=A`các hoạt động trong biểu diễn chuẩn hóa, thay vì thực hiện\(10^{18}\)bổ sung riêng lẻ. Khi các phần lẻ còn lại được phơi bày, bước bổ sung liên tục sẽ giảm một nửa chênh lệch liên quan. Đây chính xác là tình huống mà cách tiếp cận cộng trực tiếp sẽ thất bại nhưng việc xây dựng dựa trên tính chẵn lẻ vẫn nằm trong giới hạn hoạt động. 

Vì`999999999999999999 1000000000000000000`, các giá trị liền kề nhau nhưng có tính chẵn lẻ ngược lại. Giai đoạn loại bỏ tính chẵn lẻ đầu tiên ngay lập tức thay đổi giá trị chuẩn hóa bằng việc giảm một nửa ảo. Thuật toán không bao giờ giả định rằng các giá trị ban đầu đều là số lẻ, chỉ có điều chúng đều là số lẻ sau các vòng lặp loại bỏ chẵn lẻ. Điều kiện biên này ngăn chặn việc áp dụng sai phép chuyển đổi lẻ-cộng-lẻ. 

Vì`1 1`, đầu vào tối thiểu có thể, câu trả lời lại là 0. Vì`1000000000000000000 1000000000000000000`, đầu vào bằng nhau tối đa có thể hoạt động giống hệt nhau. Độ lớn của các số nguyên không có tác dụng khi đã có sự bằng nhau. 
:::
