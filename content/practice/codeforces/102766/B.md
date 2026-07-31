---
title: "CF 102766B - Singhal và Bình đẳng"
description: "Chuỗi chứa các chữ cái viết thường và mục tiêu là sửa đổi nó cho đến khi mọi chữ cái xuất hiện có tần số giống hệt nhau. Một thao tác thay đổi một ký tự hiện có thành bất kỳ ký tự chữ thường nào khác. Nhiệm vụ là tìm số lượng thay đổi nhỏ nhất cần thiết."
date: "2026-07-30T04:17:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "B"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 78
verified: true
draft: false
---

[CF 102766B - Singhal và Bình đẳng](https://codeforces.com/problemset/problem/102766/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chuỗi chứa các chữ cái viết thường và mục tiêu là sửa đổi nó cho đến khi mọi chữ cái xuất hiện có tần số giống hệt nhau. Một thao tác thay đổi một ký tự hiện có thành bất kỳ ký tự chữ thường nào khác. Nhiệm vụ là tìm số lượng thay đổi nhỏ nhất cần thiết. 

Ví dụ: nếu một chuỗi có số lượng`a = 5`,`b = 2`, Và`c = 1`, chúng ta được phép loại bỏ một số chữ cái bằng cách chuyển đổi chúng thành các chữ cái hiện có khác hoặc tạo các chữ cái mới. Chuỗi cuối cùng có thể chứa bất kỳ số lượng ký tự riêng biệt nào, nhưng mọi ký tự còn lại phải có cùng số lượng. 

Đầu vào chứa một số chuỗi độc lập. Số lượng ca kiểm thử có thể lớn tới mức`10^5`và tổng của tất cả độ dài chuỗi cũng nhiều nhất là`10^5`. Điều này có nghĩa là giải pháp phải gần tuyến tính trong tổng kích thước đầu vào. Một giải pháp thử nhiều sửa đổi hoặc quét tất cả các chuỗi được chuyển đổi có thể là không thể, trong khi cách tiếp cận thực hiện một lượng nhỏ công việc cho mỗi ký tự sẽ dễ dàng phù hợp. 

Một số trường hợp cần được chăm sóc. Nếu tất cả các ký tự đã có tần số bằng nhau thì không cần thực hiện thao tác nào. Đối với đầu vào:```
1
aabb
```câu trả lời là`0`, vì cả hai chữ cái đều đã xuất hiện hai lần. Một phương thức luôn cố gắng hợp nhất mọi thứ thành một ký tự sẽ trả về không chính xác`2`. 

Một chuỗi có một ký tự là một trường hợp ranh giới khác:```
1
aaaa
```Câu trả lời là`0`. Chuỗi đã có chính xác một ký tự riêng biệt và điều kiện được thỏa mãn vì tất cả các ký tự riêng biệt, nghĩa là chỉ`a`, có cùng tần số. 

Một trường hợp ít rõ ràng hơn là khi câu trả lời hay nhất giới thiệu số lượng ký tự riêng biệt khác với chuỗi gốc. Vì:```
1
aba
```câu trả lời là`1`. Chúng ta có thể thay đổi`b`vào trong`a`và nhận được`aaa`, hoặc thay đổi một`a`vào trong`c`và nhận được`abc`. Giải pháp chỉ cố gắng cân bằng các chữ cái hiện có sẽ bỏ lỡ khả năng thêm hoặc xóa các ký tự riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đoán số lượng ký tự riêng biệt cuối cùng, các chữ cái còn lại và tần suất mục tiêu của chúng. Đối với mỗi lần đoán, chúng ta có thể đếm xem có bao nhiêu ký tự đã khớp với cách sắp xếp cuối cùng mong muốn và thay đổi phần còn lại. Điều này đúng vì mọi trạng thái cuối cùng có thể đều được xem xét, nhưng số lượng trạng thái có thể có lại quá lớn. 

Quan sát quan trọng xuất phát từ thực tế là chỉ có 26 chữ cái viết thường. Số ký tự riêng biệt cuối cùng phải là ước số nào đó của độ dài chuỗi, vì nếu có`k`các ký tự và mỗi ký tự xuất hiện`x`lần thì`k * x = |S|`. Từ`k`không thể vượt quá 26, chúng ta chỉ cần kiểm tra một số rất nhỏ các khả năng. 

Giả sử chúng ta chọn`k`những ký tự cuối cùng. Tần số yêu cầu của chúng là`|S| / k`. Một ký tự được chọn với tần số hiện tại`c`có thể đóng góp nhiều nhất`min(c, |S| / k)`các ký tự không thay đổi. Nếu chúng ta giữ một ký tự có nhiều hơn tần số mục tiêu thì các bản sao bổ sung phải được thay đổi. Nếu nó có ít bản sao hơn, chúng ta phải tạo thêm bản sao bằng cách sử dụng các thay đổi từ các ký tự khác. 

Đối với một cố định`k`, sự lựa chọn tốt nhất là chọn`k`những lá thư có sự đóng góp lớn nhất có thể. Sau khi tính toán số lượng vị trí tối đa có thể không thay đổi, các vị trí còn lại chính xác là những thao tác mà chúng ta cần. 

Brute-force hoạt động vì nó kiểm tra các cấu hình cuối cùng có thể có, nhưng nó thất bại vì nó xử lý 26 chữ cái như thể chúng tạo ra một không gian tìm kiếm khổng lồ. Nhận xét rằng chỉ có số cuối cùng của các chữ cái riêng biệt mới quan trọng làm giảm bài toán xuống còn kiểm tra tối đa 26 trường hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng trạng thái cuối cùng có thể có | O(26) | Quá chậm | 
| Tối ưu | O(26 × 26) mỗi chuỗi | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi chữ cái viết thường trong chuỗi. Chỉ cần 26 bộ đếm vì kích thước bảng chữ cái là cố định. 
2. Hãy thử mọi số lượng ký tự riêng biệt cuối cùng có thể`k`từ`1`ĐẾN`26`. Bỏ qua các giá trị của`k`không chia độ dài chuỗi, vì các tần số bằng nhau không thể tổng bằng tổng chiều dài. 
3. Đối với hợp lệ`k`, tính tần số mục tiêu`need = length / k`. Với mỗi chữ cái, hãy tính xem có bao nhiêu lần xuất hiện hiện tại của nó có thể không thay đổi nếu chữ cái này là một trong những chữ cái cuối cùng.`k`các chữ cái. Giá trị này là`min(current_frequency, need)`. 
4. Sắp xếp 26 giá trị đóng góp này và lấy giá trị lớn nhất`k`của họ. Những điều này đại diện cho điều tốt nhất`k`các chữ cái để giữ trong chuỗi cuối cùng. 
5. Số lượng vị trí không thay đổi là tổng của những đóng góp được lựa chọn này. Các hoạt động cần thiết cho sự lựa chọn này là`length - unchanged_positions`. Giữ giá trị tối thiểu trong số tất cả hợp lệ`k`. 

Tại sao nó hoạt động: đối với bất kỳ số ký tự cuối cùng nào được chọn, mỗi ký tự cuối cùng phải có chính xác`need`lần xuất hiện. Một nhân vật không thể bảo tồn nhiều hơn`need`về những lần xuất hiện cũ của nó và việc bảo tồn ít hơn sẽ không bao giờ có lợi vì một nhân vật khác có thể sử dụng những vị trí được bảo tồn đó. Lựa chọn lớn nhất`k`đóng góp mang lại số lượng vị trí tối đa có thể tồn tại không thay đổi cho mục tiêu đó. Vì mọi số ký tự cuối cùng có thể hợp lệ đều được kiểm tra nên câu trả lời tối thiểu tìm được là câu trả lời tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()
        n = len(s)

        cnt = [0] * 26
        for ch in s:
            cnt[ord(ch) - ord('a')] += 1

        best = n

        for k in range(1, 27):
            if n % k != 0:
                continue

            need = n // k
            keep = []

            for c in cnt:
                keep.append(min(c, need))

            keep.sort(reverse=True)
            unchanged = sum(keep[:k])

            best = min(best, n - unchanged)

        ans.append(str(best))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng tần số lưu trữ trạng thái hoàn chỉnh của đầu vào vì chỉ số lượng chữ cái mới ảnh hưởng đến câu trả lời. Vòng lặp kết thúc`k`kiểm tra mọi kích thước có thể có của bảng chữ cái cuối cùng. Các giá trị không thể chia độ dài chuỗi sẽ bị bỏ qua vì chúng không thể tạo thành các nhóm bằng nhau. 

Đối với mỗi hợp lệ`k`, mã sẽ xây dựng số lượng mà mỗi chữ cái có thể đóng góp mà không cần sửa đổi. Sắp xếp đặt các chữ cái hữu ích nhất lên đầu tiên, vì vậy hãy lấy chữ cái đầu tiên`k`các giá trị cung cấp bộ ký tự tốt nhất để giữ. Câu trả lời là tổng chiều dài trừ đi các vị trí được bảo toàn này. 

Không có vấn đề về lập chỉ mục vì mọi ký tự đều được chuyển đổi thành giá trị từ`0`ĐẾN`25`. Số nguyên Python cũng loại bỏ mọi lo ngại về tràn, mặc dù giá trị thực tế rất nhỏ. 

## Ví dụ đã hoạt động 

Đối với chuỗi`aba`: 

| k | tần số mục tiêu | đóng góp | không thay đổi | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2, 1, 0, ... | 2 | 1 | 
| 3 | 1 | 1, 1, 0, ... | 2 | 1 | 

Câu trả lời tốt nhất là`1`. Dấu vết cho thấy kết quả tối ưu không cần giữ nguyên số lượng ký tự riêng biệt như chuỗi gốc. 

Đối với chuỗi`abbbc`: 

| k | tần số mục tiêu | đóng góp | không thay đổi | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 3, 1, 1, 0, ... | 3 | 2 | 
| 5 | 1 | 1, 1, 1, 0, ... | 3 | 2 | 

Câu trả lời là`2`. Giữ một ký tự yêu cầu thay đổi hai chữ cái còn lại, trong khi giữ ba ký tự khác nhau yêu cầu giảm tần suất`b`và tạo các bản sao bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 × 26) cho mỗi trường hợp thử nghiệm | Có nhiều nhất 26 cách lựa chọn`k`và sắp xếp 26 giá trị là công việc liên tục. | 
| Không gian | O(26) | Chỉ có mảng tần số bảng chữ cái và các giá trị đóng góp tạm thời được lưu trữ. | 

Tổng kích thước đầu vào được giới hạn ở`10^5`các ký tự, do đó khối lượng công việc không đổi được thực hiện cho mỗi chuỗi dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()
        n = len(s)

        cnt = [0] * 26
        for ch in s:
            cnt[ord(ch) - 97] += 1

        best = n

        for k in range(1, 27):
            if n % k == 0:
                need = n // k
                values = [min(x, need) for x in cnt]
                values.sort(reverse=True)
                best = min(best, n - sum(values[:k]))

        ans.append(str(best))

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

assert solve("""6
aba
abba
abbc
abbbc
codedigger
codealittle
""") == """1
0
1
2
2
3
"""

assert solve("""1
a
""") == """0
"""

assert solve("""1
aaaa
""") == """0
"""

assert solve("""1
abcde
""") == """0
"""

assert solve("""1
aaaaab
""") == """1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0`| Kích thước chuỗi tối thiểu và ký tự riêng biệt duy nhất | 
|`aaaa`|`0`| Đã có tần số bằng nhau | 
|`abcde`|`0`| Mỗi ký tự đã có tần số một | 
|`aaaaab`|`1`| Xóa số lượng ký tự được thể hiện quá mức | 

## Vỏ cạnh 

cho`aabb`, thuật toán kiểm tra`k = 1`Và`k = 2`. Khi`k = 2`, tần số mục tiêu là`2`, cả hai chữ cái đều đóng góp`2`, và cả bốn vị trí đều không thay đổi. Câu trả lời là`0`, tránh mắc lỗi buộc chuỗi thành một ký tự. 

Vì`aaaa`, chỉ có một chữ cái tồn tại, nhưng`k = 1`hợp lệ vì độ dài chia hết cho một. Tần số mục tiêu là`4`, và chữ cái đóng góp cả bốn vị trí. Thuật toán trả về`0`. 

Vì`aba`, thuật toán xem xét cả hai`k = 1`Và`k = 3`. Với`k = 1`, đóng góp tốt nhất là hai không thay đổi`a`nhân vật. Với`k = 3`, những đóng góp tốt nhất là một`a`và một`b`, để lại một ký tự bị thiếu để tạo. Cả hai lựa chọn đều yêu cầu một thao tác, vì vậy câu trả lời cuối cùng là`1`. 

Vì`abbbc`, các lựa chọn hợp lệ là`k = 1`Và`k = 5`. Lựa chọn đầu tiên giữ lại ba`b`ký tự và thay đổi hai ký tự còn lại. Lựa chọn thứ hai giữ một bản sao của mỗi ký tự có sẵn và thay đổi hai vị trí để tạo ra các chữ cái còn thiếu. Cả hai đều dẫn đến hai thao tác, điều này khẳng định thuật toán xử lý các trường hợp loại bỏ ký tự và thêm ký tự đều tốt như nhau.
