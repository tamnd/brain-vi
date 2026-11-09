---
title: "CF 1490B - Số dư cân bằng"
description: "Chúng ta được cho một mảng có độ dài luôn chia hết cho ba. Đối với mọi số, chỉ có modulo 3 còn lại của nó là quan trọng. Một bước di chuyển sẽ tăng một phần tử mảng lên 1."
date: "2026-06-10T22:39:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1490
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 702 (Div. 3)"
rating: 1000
weight: 1490
solve_time_s: 310
verified: true
draft: false
---

[CF 1490B - Số dư cân bằng](https://codeforces.com/problemset/problem/1490/B) 

**Đánh giá:** 1000 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng có độ dài luôn chia hết cho ba. Đối với mọi số, chỉ có modulo 3 còn lại của nó là quan trọng. 

Một bước di chuyển sẽ làm tăng một phần tử mảng lên 1. Từ góc độ số dư, việc tăng một số lên 1 sẽ di chuyển phần tử đó dọc theo chu kỳ: 

0 → 1 → 2 → 0 

Mục tiêu của chúng ta là đếm các số có số dư 0, 1 và 2 đều bằng nhau. Vì độ dài mảng chia hết cho 3 nên mỗi lớp còn lại phải chứa chính xác`n / 3`các phần tử. 

Các giá trị thực tế trong mảng phần lớn không liên quan. Thông tin duy nhất ảnh hưởng đến câu trả lời là có bao nhiêu số hiện thuộc về mỗi lớp còn lại. Nếu chúng ta biết số lượng`c[0]`,`c[1]`,`c[2]`thì nhiệm vụ sẽ trở thành cân bằng ba nhóm này bằng cách sử dụng các lần chuyển rẻ nhất có thể xung quanh biểu đồ số dư tròn. 

Những ràng buộc rất thân thiện. Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm tối đa là 150.000. Bất kỳ thuật toán nào xử lý từng phần tử với số lần không đổi đều dễ dàng đủ nhanh. Thậm chí một`O(n)`giải pháp cho mỗi trường hợp thử nghiệm là tầm thường trong giới hạn. Không cần lập trình động, thuật toán đồ thị hoặc cấu trúc dữ liệu nặng. 

Trường hợp cạnh tinh vi xuất hiện khi tất cả các phần tử dư thừa phải di chuyển qua nhiều lớp còn lại. 

Coi như:```
n = 6
remainders: [0,0,0,0,0,0]
```Số lượng là:```
c0 = 6, c1 = 0, c2 = 0
target = 2
```Câu trả lời đúng là 6. 

Hai phần tử di chuyển từ lớp 0 đến lớp 1, tốn tổng cộng 2 lần di chuyển. Hai phần tử nữa phải chuyển từ lớp 0 sang lớp 2. Vì không có thao tác trực tiếp nào từ số dư 0 đến số dư 2 nên mỗi lần chuyển như vậy tốn 2 lượt, đóng góp thêm 4 lượt. Tổng số là 6. 

Giải pháp bất cẩn chỉ chuyển trực tiếp vào các lớp thiếu sẽ tính sai 4. 

Một lỗi dễ mắc phải khác là dừng lại sau một lần giữ thăng bằng. 

Coi như:```
n = 6
remainders: [2,2,2,2,2,2]
```Đếm:```
c0 = 0, c1 = 0, c2 = 6
target = 2
```Việc di chuyển hai phần tử từ lớp 2 sang lớp 0 tốn 2 lần di chuyển mỗi phần tử, tạo ra:```
c0 = 2, c1 = 0, c2 = 4
```Mảng vẫn chưa cân bằng. Lớp 2 vẫn còn dư thừa và phải gửi thêm hai phần tử nữa đến lớp 1. Câu trả lời cuối cùng là 6. Việc phân phối lại một lần sẽ bỏ lỡ sự điều chỉnh thứ hai này. 

Trường hợp cạnh thứ ba xảy ra khi mảng đã được cân bằng.```
n = 6
a = [0,1,2,3,4,5]
```Đếm:```
c0 = c1 = c2 = 2
```Câu trả lời là 0. Bất kỳ giải pháp nào thực hiện chuyển khoản một cách mù quáng mà không kiểm tra số lượng dư thừa đều có thể thêm các bước di chuyển không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng các hoạt động riêng lẻ. Bất cứ khi nào một số lớp còn lại chứa quá nhiều phần tử, hãy chọn một phần tử từ lớp đó và liên tục tăng nó cho đến khi đạt đến lớp bị thiếu. 

Cách tiếp cận này đúng vì nó tuân theo chính xác các hoạt động được phép. Vấn đề là chúng ta đang nghĩ về những con số riêng lẻ khi chỉ có con số mới quan trọng. Trong các biến thể lớn hơn của ý tưởng này, chúng tôi có thể thực hiện nhiều mô phỏng không cần thiết. Việc theo dõi các yếu tố thực tế không cung cấp thêm thông tin nào. 

Quan sát quan trọng là mọi thao tác chỉ thay đổi một lớp còn lại. Tăng một số một lần sẽ di chuyển một mục khỏi lớp`r`đến lớp`(r+1) mod 3`. 

Thay vì thao tác các phần tử, chúng ta có thể thao tác số lượng. 

Giả sử lớp`i`hiện chứa nhiều hơn số lượng mục tiêu. Sự dư thừa```
c[i] - target
```cuối cùng phải rời khỏi lớp học này. Đích rẻ nhất luôn là lớp còn lại tiếp theo trong chu kỳ, bởi vì một phần tăng thực hiện chính xác quá trình chuyển đổi đó. 

Bất cứ khi nào một lớp có phần tử thừa, chúng tôi chuyển tất cả phần thừa sang lớp tiếp theo, trả một nước đi cho mỗi phần tử được chuyển và cập nhật số lượng. Điều này có thể tạo ra sự dư thừa ở lớp tiếp theo và sẽ được xử lý sau. 

Vì chỉ có ba lớp còn lại nên việc truyền lặp đi lặp lại phần dư thừa trong chu kỳ sẽ nhanh chóng đạt đến trạng thái cân bằng. Chạy qua lớp vài lần là đủ. Giải pháp tiêu chuẩn thực hiện sáu lần lặp, đảm bảo rằng bất kỳ phần dư thừa nào cũng có thể di chuyển tối đa hai vòng trong một chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng các phần tử Brute Force | O(n + câu trả lời) | O(n) | Chậm một cách không cần thiết | 
| Đếm phân phối lại | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu phần tử mảng có số dư 0, 1 và 2 modulo 3. 
2. Tính quy mô mục tiêu của từng lớp:```
target = n / 3
```3. Khởi tạo câu trả lời về 0. 
4. Lặp lại các lớp còn lại vài lần. Sáu lần lặp là đủ vì chỉ có ba lớp và phần dư có thể cần phải di chuyển quanh chu kỳ nhiều lần. 
5. Đối với mỗi lớp`i`, kiểm tra xem`c[i] > target`. 
6. Nếu quá mức, hãy để```
extra = c[i] - target
```di chuyển tất cả những yếu tố đó vào lớp`(i + 1) % 3`. 
7. Mỗi phần tử được chuyển yêu cầu một phần tử, vì vậy hãy thêm`extra`để trả lời. 
8. Cập nhật số lượng:```
c[i] -= extra
c[(i + 1) % 3] += extra
```9. Sau tất cả các lần vượt qua, số lượng được cân bằng và chi phí tích lũy là số lần di chuyển tối thiểu. 

### Tại sao nó hoạt động 

Hoạt động duy nhất được phép di chuyển một phần tử khỏi lớp còn lại`r`đến lớp còn lại`(r+1) mod 3`và chi phí chính xác là một lần di chuyển. Bất cứ khi nào một lớp chứa nhiều hơn số phần tử mục tiêu, cuối cùng mọi phần tử dư thừa phải rời khỏi lớp đó. Gửi phần thừa trực tiếp sang lớp tiếp theo là hành động rẻ nhất có thể vì đó chính xác là những gì một phần tăng thêm thực hiện. 

Thuật toán duy trì tính bất biến là không có phần tử nào bị di chuyển trừ khi nó thuộc về lớp dư thừa. Mỗi lần chuyển nhượng sẽ giảm bớt phần thặng dư của lớp hiện tại càng nhiều càng tốt. Nếu lớp nhận trở nên quá mức thì phần vượt quá đó sẽ được truyền đi sau đó. Vì thặng dư chỉ có thể di chuyển tiếp theo trong chu kỳ và mỗi bước di chuyển được tính chính xác một lần nên tổng chi phí bằng số lượng gia tăng tối thiểu cần thiết để đạt được số lượng cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0, 0, 0]
        for x in a:
            cnt[x % 3] += 1

        target = n // 3
        moves = 0

        for i in range(6):
            r = i % 3

            if cnt[r] > target:
                extra = cnt[r] - target
                cnt[r] -= extra
                cnt[(r + 1) % 3] += extra
                moves += extra

        ans.append(str(moves))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính ba số còn lại. Điều này làm giảm vấn đề từ nhiệm vụ cân bằng mảng sang nhiệm vụ cân bằng ba nhóm. 

Biến`target`lưu trữ kích thước yêu cầu của mỗi thùng. Từ`n`chia hết cho ba, không tồn tại vấn đề làm tròn. 

Vòng phân phối lại là cốt lõi của giải pháp. Bất cứ khi nào một lớp chứa nhiều hơn`target`các phần tử, tất cả phần thừa sẽ được đẩy về phía trước một bước còn lại. Mỗi phần tử được truyền tương ứng với chính xác một thao tác tăng dần, vì vậy`moves`tăng lên cùng một lượng. 

Vòng lặp chạy sáu lần thay vì ba lần. Một lượt hoàn thành có thể tạo ra thặng dư mới trong lớp tiếp theo. Đường chuyền thứ hai cho phép phần dư đó tiếp tục di chuyển. Chỉ với ba lớp còn lại, sáu lần lặp là đủ để tất cả phần thừa được lan truyền hoàn toàn. 

Không tồn tại mối lo ngại về tràn số nguyên vì câu trả lời tối đa nhỏ so với phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
0 2 5 5 4 8
```Số đếm ban đầu:```
c0 = 1
c1 = 1
c2 = 4
target = 2
```| Bước | Đếm (c0,c1,c2) ​​| Thêm cảm động | Chi phí tăng thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | (1,1,4) | - | - | 0 | 
| r=2 | (2,1,3) | 1 | 1 | 1 | 
| r=2 nữa | (3,1,2) | 1 | 1 | 2 | 
| r=0 | (2,2,2) | 1 | 1 | 3 | 

Câu trả lời cuối cùng:```
3
```Ví dụ này cho thấy thặng dư liên tục lan truyền trong chu kỳ. Phần thừa của lớp 2 trước tiên sẽ tạo ra các phần tử bổ sung ở lớp 0, sau đó lớp 0 mới có thể tặng cho lớp 1. 

### Mẫu 2 

đầu vào:```
6
2 0 2 1 0 0
```Phần còn lại:```
2,0,2,1,0,0
```Đếm:```
c0 = 3
c1 = 1
c2 = 2
target = 2
```| Bước | Đếm (c0,c1,c2) ​​| Thêm cảm động | Chi phí tăng thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | (3,1,2) | - | - | 0 | 
| r=0 | (2,2,2) | 1 | 1 | 1 | 

Câu trả lời cuối cùng:```
1
```Trường hợp này thể hiện tình huống đơn giản nhất trong đó một lớp thừa trực tiếp sửa một lớp thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số dư chiếm ưu thế trong công việc | 
| Không gian | O(1) | Chỉ có ba quầy được lưu trữ | 

Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm tối đa là 150.000. Việc xử lý từng phần tử một lần dễ dàng trong thời gian giới hạn và bộ nhớ phụ không đổi không đáng kể so với giới hạn bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0, 0, 0]
        for x in a:
            cnt[x % 3] += 1

        target = n // 3
        moves = 0

        for i in range(6):
            r = i % 3
            if cnt[r] > target:
                extra = cnt[r] - target
                cnt[r] -= extra
                cnt[(r + 1) % 3] += extra
                moves += extra

        out.append(str(moves))

    return "\n".join(out)

# provided sample
assert run(
"""4
6
0 2 5 5 4 8
6
2 0 2 1 0 0
9
7 1 3 4 2 10 3 9 6
6
0 1 2 3 4 5
"""
) == """3
1
3
0"""

# minimum size, already balanced
assert run(
"""1
3
0 1 2
"""
) == "0"

# all elements in remainder class 0
assert run(
"""1
6
0 3 6 9 12 15
"""
) == "6"

# all elements in remainder class 2
assert run(
"""1
6
2 5 8 11 14 17
"""
) == "6"

# one surplus bucket fixing one deficit
assert run(
"""1
6
0 0 0 1 1 2
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 1 2`|`0`| Kích thước hợp lệ tối thiểu và đã được cân bằng | 
| Sáu bội số của 3 |`6`| Tuyên truyền nhiều bước trong suốt chu trình | 
| Sáu số còn lại 2 |`6`| Thặng dư quấn quanh modulo 3 | 
|`0 0 0 1 1 2`|`1`| Chuyển khoản trực tiếp một lần | 

## Vỏ cạnh 

Hãy xem xét:```
1
6
0 3 6 9 12 15
```Tất cả các số đều thuộc lớp dư 0. 

Số đếm ban đầu:```
(6,0,0)
```Mục tiêu:```
2
```Thuật toán di chuyển bốn phần tử dư thừa từ lớp 0 sang lớp 1, có giá 4. Số lượng trở thành:```
(2,4,0)
```Lớp 1 bây giờ dư 2, chuyển sang lớp 2 với chi phí thêm 2. Số đếm cuối cùng:```
(2,2,2)
```Tổng số câu trả lời:```
6
```Điều này khẳng định rằng lượng dư thừa có thể cần phải chuyển qua các lớp trung gian. 

Coi như:```
1
6
2 5 8 11 14 17
```Số đếm ban đầu:```
(0,0,6)
```Lần phân phối lại đầu tiên sẽ gửi bốn phần tử từ lớp 2 đến lớp 0:```
(4,0,2)
```Chi phí trở thành 4. 

Tiếp theo, lớp 0 gửi hai phần tử thừa đến lớp 1:```
(2,2,2)
```Chi phí trở thành 6. 

Điều này chứng tỏ tại sao cần phải có nhiều lượt đi. Cân bằng một giai cấp có thể tạo ra thặng dư mới ở nơi khác. 

Coi như:```
1
6
0 1 2 3 4 5
```Số đếm ban đầu:```
(2,2,2)
```Không có lớp nào vượt mục tiêu. Vòng lặp phân phối lại không thực hiện chuyển giao và câu trả lời vẫn là 0. Điều này xác nhận rằng thuật toán không đưa ra các bước di chuyển không cần thiết khi mảng đã được cân bằng.
