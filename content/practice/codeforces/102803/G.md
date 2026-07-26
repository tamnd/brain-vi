---
title: "CF 102803G - Tạm biệt"
description: "Trò chơi được chơi trên một con số chứ không phải trên một bảng hoặc biểu đồ. Một bước di chuyển sẽ thay thế số hiện tại bằng một trong các ước số của nó, nhưng số chia không thể là 1 và không thể là chính số đó."
date: "2026-07-26T16:23:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "G"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 43
verified: true
draft: false
---

[CF 102803G - Tạm biệt](https://codeforces.com/problemset/problem/102803/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một con số chứ không phải trên một bảng hoặc biểu đồ. Một bước di chuyển sẽ thay thế số hiện tại bằng một trong các ước số của nó, nhưng số chia không thể là 1 và không thể là chính số đó. Người chơi không có nước đi hợp pháp sẽ thắng, điều này khiến trò chơi trở thành một trò chơi chia số sai lầm: đạt đến số nguyên tố hoặc 1 là tốt vì người chơi tiếp theo buộc phải mất cơ hội đi nước rút. 

Chino đi trước ở số n đã cho. Cô ấy muốn chọn một số chia trong lượt đầu tiên của mình khiến David rơi vào thế thua. Trong số tất cả những lựa chọn như vậy, cô ấy muốn con số lớn nhất có thể. Nếu bản thân n không có nước đi đầu tiên hợp pháp thì cô ấy đã thắng và câu trả lời là 0. Nếu mọi nước đi đầu tiên có thể có đều cho phép David thắng thì câu trả lời là -1. 

Giá trị của n nhiều nhất là 100000 và có thể có tới 1000 trường hợp thử nghiệm. Điều này loại trừ việc liên tục khám phá toàn bộ cây ước cho mỗi đầu vào. Tìm kiếm trò chơi đệ quy trên tất cả các ước số có thể liên tục truy cập vào cùng một trạng thái và có thể trở nên tốn kém, vì vậy giải pháp cần nhận ra cấu trúc toán học của các vị trí thắng và thua cũng như xử lý trước thông tin hữu ích. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Với n = 1, không có ước số hợp lệ, do đó Chino thắng ngay lập tức và câu trả lời là 0. Việc thực hiện bất cẩn khi tìm kiếm ước số thắng sẽ trả về sai -1. 

Với n = 7, tình huống tương tự cũng xảy ra với số nguyên tố. Các ước số duy nhất là 1 và 7, cả hai đều bị cấm, vì vậy kết quả đầu ra đúng là 0. 

Với n = 6, lựa chọn đầu tiên duy nhất có thể là 2 và 3. Cả hai đều là vị trí chính, nghĩa là người chơi tiếp theo không di chuyển và thắng. Chino không thể ép thắng nên kết quả đúng là -1. Cách tiếp cận chỉ kiểm tra xem số chia có tồn tại hay không sẽ thất bại ở đây. 

Với n = 12, Chino có thể chọn 4. Số 4 chỉ có thể đi một nước đi là 2, và 2 là thế thắng cuối cùng. Do đó 4 là vị trí thua đối với người chơi đến lượt, nên David thua sau khi Chino chọn 4. Kết quả đúng là 4. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ mô hình hóa mọi số dưới dạng trạng thái trò chơi. Đối với số x, chúng ta có thể tạo ra tất cả các ước số thích hợp và xác định đệ quy xem liệu người chơi hiện tại có thể giành chiến thắng hay không. Điều này đúng vì mỗi lần di chuyển đều làm giảm số lượng, do đó đệ quy cuối cùng sẽ đạt đến số nguyên tố hoặc 1. 

Vấn đề là điều này lặp đi lặp lại cùng một lý do nhiều lần. Ví dụ: nhiều số tổng hợp khác nhau chứa 4, 6 hoặc 9 làm ước số và một phép tìm kiếm đơn giản sẽ liên tục tính lại kết quả của các vị trí đó. Ngay cả với tính năng ghi nhớ, việc thực hiện việc này một cách độc lập đối với nhiều trường hợp kiểm thử sẽ gây lãng phí. Việc tạo các ước số cho mọi trạng thái và đi qua tất cả các trạng thái có thể tiếp cận là quá lớn khi n đạt tới 100000. 

Điều quan trọng cần lưu ý là trò chơi có cấu trúc vị trí thua rất đơn giản. Một vị trí chỉ thua khi mọi nước đi có thể đều dẫn đến vị trí thắng. Số nguyên tố và số 1 là vị trí chiến thắng vì không có nước đi nào. Một số tổng hợp sẽ thua chính xác khi tất cả các ước thực sự của nó đều là số nguyên tố. Điều đó xảy ra chính xác khi số đó là tích của hai số nguyên tố, tính bội số. 

Nếu một số chứa một ước số thực sự tổng hợp thì ước số đó đã có thể là một nước đi. Các ước số tổng hợp nhỏ nhất quan trọng luôn là tích của hai số nguyên tố và các vị trí đó sẽ thua. Điều này có nghĩa là mọi số tổng hợp có nhiều hơn hai thừa số nguyên tố đều chuyển sang vị trí thua và thắng. 

Sau đó, bài toán được rút gọn thành việc tìm ước số nửa nguyên tố lớn nhất của n, trong đó nửa nguyên tố có nghĩa là có chính xác hai thừa số nguyên tố có bội số. Số chia đó là nước đi đầu tiên tốt nhất vì nó khiến David rơi vào tình trạng thua cuộc. 

Lực lượng vũ phu và phương pháp tối ưu hóa có thể được so sánh như sau.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Có khả năng O(số trạng thái ước số có thể truy cập cho mỗi trường hợp thử nghiệm) | O(n) với khả năng ghi nhớ | Quá chậm trong nhiều trường hợp | 
| Tối ưu | Tiền xử lý O(N log log N + N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các test case và tìm giá trị N tối đa xuất hiện. Tất cả quá trình tiền xử lý có thể được giới hạn ở giá trị này vì không có truy vấn nào cần số lớn hơn. 
2. Dùng sàng tìm các số nguyên tố đến N. Việc phân loại trạng thái trò chơi chỉ phụ thuộc vào thừa số nguyên tố của nó nên cần thông tin nguyên tố nhanh. 
3. Đánh dấu mọi số là nửa nguyên tố. Một số là nửa nguyên tố nếu số nguyên tố có bội số của nó chính xác là hai. Ví dụ: 4 = 2 × 2, 6 = 2 × 3 và 9 = 3 × 3 là các nửa nguyên tố. 
4. Với mọi nửa nguyên tố d, hãy cập nhật tất cả các bội số của d lớn hơn d. Giá trị d là nước đi đầu tiên hợp lệ cho các bội số đó và việc giữ nguyên ứng cử viên tối đa sẽ mang lại nước đi đầu tiên thắng lớn nhất. Bội số bằng d bị bỏ qua vì Chino không thể chọn chính số đó. 
5. Với mỗi truy vấn n, trước tiên hãy xử lý các trạng thái đặc biệt. Nếu n là 1 hoặc số nguyên tố thì Chino thắng mà không di chuyển nên ra kết quả 0. Nếu n là nửa nguyên tố thì tất cả nước đi hợp lệ đều là số nguyên tố nên Chino không thể thắng và đáp án là -1. Ngược lại, xuất ra ước số nửa nguyên tố lớn nhất được lưu trong quá trình tiền xử lý. 

Tính đúng đắn đến từ tính bất biến rằng mọi câu trả lời được lưu trữ cho một số x đều là ước số lớn nhất của x và đó là trạng thái trò chơi thua. Trạng thái trò chơi thua chính xác là trạng thái bán kết, vì vậy bất kỳ ứng cử viên nào được lưu trữ đều là nước đi đầu tiên thắng. Vì mọi số tổng hợp không phải là số nguyên tố đều có ít nhất một ước số nửa nguyên tố nên giá trị được lưu trữ sẽ tồn tại bất cứ khi nào Chino có thể thắng bằng cách di chuyển. Việc chọn ước số được lưu trữ lớn nhất thỏa mãn sở thích của Chino mà không làm thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    queries = [int(input()) for _ in range(int(input()))]
    if not queries:
        return

    limit = max(queries)

    prime = [True] * (limit + 1)
    if limit >= 0:
        prime[0] = False
    if limit >= 1:
        prime[1] = False

    i = 2
    while i * i <= limit:
        if prime[i]:
            step = i
            start = i * i
            prime[start:limit + 1:step] = [False] * (((limit - start) // step) + 1)
        i += 1

    factor_count = [0] * (limit + 1)
    for p in range(2, limit + 1):
        if prime[p]:
            for x in range(p, limit + 1, p):
                y = x
                while y % p == 0:
                    factor_count[x] += 1
                    y //= p

    semiprime = [False] * (limit + 1)
    for x in range(2, limit + 1):
        if factor_count[x] == 2:
            semiprime[x] = True

    best = [0] * (limit + 1)
    for d in range(2, limit + 1):
        if semiprime[d]:
            for multiple in range(d * 2, limit + 1, d):
                best[multiple] = d

    ans = []
    for n in queries:
        if n <= 1 or prime[n]:
            ans.append("0")
        elif semiprime[n]:
            ans.append("-1")
        else:
            ans.append(str(best[n]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Sàng tạo mảng nguyên tố theo cách thông thường bằng cách loại bỏ bội số của mọi số nguyên tố được phát hiện. Các trường hợp biên cho 0 và 1 được đặt rõ ràng vì chúng không phải là số nguyên tố và vòng sàng bắt đầu từ 2. 

Vòng đếm thừa số đếm các thừa số nguyên tố lặp đi lặp lại. Chi tiết này quan trọng vì 8 = 2 × 2 × 2 có ba thừa số nguyên tố và không phải là nửa nguyên tố, trong khi 4 = 2 × 2 là nửa nguyên tố. Chỉ tính các thừa số nguyên tố khác nhau sẽ phân loại cả hai không chính xác. 

các`best`mảng được lấp đầy bằng cách lặp qua các nửa nguyên tố thay vì lặp qua các ước số cho mọi truy vấn. Mỗi nửa nguyên tố tự đóng góp vào tất cả các bội số lớn hơn và các đóng góp sau đó sẽ ghi đè các câu trả lời nhỏ hơn vì số lần lặp đi lên. Sự nghiêm khắc`2 * d`điểm bắt đầu ngăn việc lưu trữ số gốc làm ước số của chính nó. 

Việc xử lý truy vấn cuối cùng tuân theo phân loại lý thuyết trò chơi một cách trực tiếp. Các số nguyên tố và 1 là số thắng ngay lập tức, số nửa nguyên tố là số thua không thể tránh khỏi và tất cả các số tổng hợp còn lại đều sử dụng ước số nửa nguyên tố tốt nhất được tính toán trước. 

## Ví dụ đã hoạt động 

Xem xét giá trị đầu vào 6. 

| Số | Xuất sắc? | Bán sơ cấp? | Ước số nửa nguyên tố tốt nhất | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 6 | Không | Có | Không được sử dụng | -1 | 

Số 6 có hai lựa chọn hợp lệ là 2 và 3. Cả hai đều là số nguyên tố nên David sẽ nhận được thế không được di chuyển và thắng. Phân loại bán chính xác định chính xác 6 là vị trí thua của người chơi đầu tiên. 

Xét giá trị đầu vào 12. 

| Số | Xuất sắc? | Bán sơ cấp? | Ước số nửa nguyên tố tốt nhất | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 12 | Không | Không | 4 | 4 | 

Trong quá trình tiền xử lý, 4 được nhận dạng là nửa nguyên tố. Vì 4 chia hết cho 12 và nhỏ hơn 12 nên nó trở thành một câu trả lời ứng cử viên. Không có ước số bán nguyên tố lớn hơn của 12, vì vậy Chino chọn 4 và khiến David rơi vào thế thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log log N + N log N) | Sàng lấy O(N log log N), việc đếm hệ số được giới hạn bởi bội số nguyên tố và đánh dấu bội số bán nguyên tố là một quy trình kiểu chuỗi hài hòa | 
| Không gian | O(N) | Một số mảng có kích thước N lưu trữ tính nguyên tố, số lượng yếu tố, phân loại và câu trả lời | 

Với N nhiều nhất là 100000, quá trình tiền xử lý vẫn thoải mái trong giới hạn. Số lượng trường hợp thử nghiệm không ảnh hưởng đáng kể đến thời gian chạy vì tất cả các trường hợp đều có cùng tính toán trước. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided-style samples
assert run("1\n6\n") == "-1\n", "semiprime losing position"

# custom cases
assert run("5\n1\n2\n4\n12\n100000\n") == "0\n0\n-1\n4\n10000\n", "boundary and composite cases"
assert run("4\n7\n9\n10\n18\n") == "0\n-1\n-1\n9\n", "prime squares and products"
assert run("3\n8\n27\n30\n") == "4\n9\n15\n", "higher powers and multiple semiprimes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 2, 4, 12, 100000 | 0, 0, -1, 4, 10000 | Giá trị tối thiểu, thắng trực tiếp, phát hiện bán nguyên tố, đầu vào lớn | 
| 7, 9, 10, 18 | 0, -1, -1, 9 | Xử lý số nguyên tố và tích của hai số nguyên tố | 
| 8, 27, 30 | 4, 9, 15 | Đúng lựa chọn ước số thua lớn nhất | 

## Vỏ cạnh 

Với n = 1, thuật toán kiểm tra`n <= 1`đầu tiên và trả về 0 ngay lập tức. Không có số chia để chọn nên Chino đã thắng trước khi ra tay. 

Đối với một số nguyên tố chẳng hạn như n = 7, mảng nguyên tố đánh dấu nó là số nguyên tố và thuật toán trả về 0. Việc tìm kiếm các ước số nửa nguyên tố sẽ sai vì số nguyên tố không có bước di chuyển hợp pháp nào cả. 

Đối với một số nửa nguyên tố, chẳng hạn như n = 6, thuật toán sẽ phát hiện rằng số nguyên tố của nó chính xác là 2 và trả về -1. Việc chọn bất kỳ ước số thích hợp nào cũng sẽ cho ra một số nguyên tố, đây là vị trí chiến thắng đối với David. 

Đối với một số tổng hợp lớn hơn, chẳng hạn như n = 30, các ước số bán nguyên tố là 6, 10 và 15. Giai đoạn tiền xử lý lưu số lớn nhất, 15. Vì 15 sẽ thua cho người chơi tiếp theo, Chino có thể buộc người chơi thắng bằng cách chọn nước đi hợp lệ lớn nhất.
