---
title: "CF 102785A - Bộ điều khiển lười biếng"
description: "Mỗi lần cân ghi hai số nguyên. Đầu tiên là số lượng các bộ phận giống hệt nhau được đặt trên cân và thứ hai là tổng trọng lượng đo được của các bộ phận đó. Nếu mọi phần trong lô có cùng trọng lượng nguyên x thì mọi bản ghi phải thỏa mãn wi = ai × x."
date: "2026-07-28T15:47:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 59
verified: true
draft: false
---

[CF 102785A - Bộ điều khiển lười biếng](https://codeforces.com/problemset/problem/102785/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi lần cân ghi hai số nguyên. Đầu tiên là số lượng các bộ phận giống hệt nhau được đặt trên cân và thứ hai là tổng trọng lượng đo được của các bộ phận đó. 

Nếu mọi phần trong lô có cùng trọng lượng nguyên`x`, thì mọi bản ghi đều phải thỏa mãn`wi = ai × x`. 

Bộ điều khiển không biết trước trọng lượng chính xác. Anh ta chỉ kiểm tra xem có tồn tại một trọng số nguyên nào đó giải thích được mọi phép đo được ghi lại hay không. Nếu tồn tại ít nhất một số nguyên như vậy thì không có bằng chứng nào về lỗi và lô hàng được chấp nhận. Nếu không có trọng lượng nguyên nào phù hợp với tất cả các phép đo thì lô hàng đó chắc chắn bị lỗi. 

Số lượng phép đo nhiều nhất là 1000. Mỗi giá trị liên quan cũng đủ nhỏ để số học số nguyên đơn giản là đủ. Không cần bất kỳ tối ưu hóa nâng cao nào vì ngay cả thuật toán xử lý mọi bản ghi một lần cũng dễ dàng nằm trong giới hạn. 

Khó khăn thực sự duy nhất là giải thích chính xác "khiếm khuyết" nghĩa là gì. Chúng tôi không cố gắng xác định trọng lượng thực sự của các bộ phận. Chúng ta chỉ cần xác định liệu một số trọng số nguyên có thể giải thích đồng thời mọi phép đo hay không. 

Một lỗi phổ biến là chỉ kiểm tra xem mỗi phép đo riêng lẻ có trọng số nguyên hay không. Coi như```
2
2 6
3 12
```Lần cân đầu tiên cho thấy trọng lượng là`3`, trong khi thứ hai gợi ý trọng số của`4`. Cả hai đều là số nguyên, nhưng chúng không đồng ý, vì vậy kết quả đúng là```
DEFECT
```Một sai lầm dễ mắc phải khác là chọn trọng số từ phép đo đầu tiên và bỏ qua xem nó có phải là số nguyên hay không.```
2
2 5
4 10
```Bản ghi đầu tiên sẽ ngụ ý trọng lượng của`2.5`, điều này là không thể vì trọng số phải là số nguyên. Mặc dù cả hai phép đo đều nhất quán với cùng một giá trị phân số nhưng câu trả lời đúng là```
DEFECT
```Trường hợp cạnh cuối cùng chỉ có một phép đo.```
1
7 35
```Trọng lượng ngụ ý duy nhất là`5`, là một số nguyên, nên kết quả đúng là```
QUALITY
```Một phép đo nhất quán là đủ vì bộ điều khiển chỉ từ chối lô khi các bản ghi mâu thuẫn với mọi trọng số nguyên có thể có. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi trọng số nguyên có thể và kiểm tra xem tất cả các phép đo có thỏa mãn`wi = ai × weight`. Điều này đúng vì lô được chấp nhận chính xác khi có ít nhất một trọng lượng ứng cử viên khớp với mọi bản ghi. 

Điểm yếu là quyết định có bao nhiêu trọng số ứng viên phải được kiểm tra. Tuyên bố không cung cấp giới hạn trên rõ ràng về trọng lượng thực. Ngay cả khi chúng tôi lấy được một giá trị từ đầu vào bằng cách xem xét mọi giá trị có thể có cho đến trọng số quan sát được lớn nhất, thì thuật toán sẽ thực hiện tới gần một triệu lần kiểm tra cho mỗi trong số một nghìn phép đo, dẫn đến khoảng một tỷ phép so sánh trong trường hợp xấu nhất. 

Quan sát quan trọng là mọi phép đo đều trực tiếp xác định trọng số duy nhất có thể mà nó có thể đại diện. Sắp xếp lại phương trình cho`weight = wi / ai`. 

Để phép đo có giá trị, phép chia phải chính xác vì trọng số là số nguyên. Mọi phép đo hợp lệ cũng phải tạo ra giá trị chính xác như nhau. Thay vì tìm kiếm thông qua các trọng số có thể có, chúng ta chỉ cần tính trọng số ngụ ý từ bản ghi đầu tiên và xác minh rằng mọi bản ghi còn lại đều có cùng một số nguyên. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính thông qua các phép đo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k × W), trong đó W là phạm vi trọng số tìm kiếm | O(1) | Quá chậm | 
| Tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lần đo. 
2. Đọc số đo đầu tiên. Nếu tổng trọng lượng của nó không chia hết cho số phần thì in ngay`DEFECT`, bởi vì không có trọng số nguyên nào có thể giải thích được ngay cả bản ghi đơn lẻ này. 
3. Mặt khác, tính trọng số ứng viên như sau`w1 // a1`. Mọi phép đo hợp lệ phải tạo ra chính xác giá trị này. 
4. Xử lý từng phép đo còn lại. 
5. Đối với mỗi phép đo, trước tiên hãy kiểm tra xem`wi`chia hết cho`ai`. Nếu không thì in`DEFECT`và chấm dứt vì trọng số ngụ ý không phải là số nguyên. 
6. Tính trọng lượng ngụ ý`wi // ai`. Nếu nó khác với trọng lượng ứng viên, hãy in`DEFECT`và chấm dứt vì các phép đo khác nhau yêu cầu trọng lượng bộ phận khác nhau. 
7. Nếu mọi phép đo đều vượt qua cả hai lần kiểm tra, hãy in`QUALITY`. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến trong suốt quá trình quét. Trọng số ứng cử viên là trọng số nguyên duy nhất phù hợp với mọi phép đo được xử lý. 

Phép đo hợp lệ đầu tiên xác định duy nhất ứng cử viên. Mọi phép đo sau này đều phù hợp với nó hoặc chứng minh rằng không tồn tại trọng số nguyên chung. Vì mọi giải pháp hợp lệ có thể phải thỏa mãn mọi phép đo, nên chỉ chấp nhận khi tất cả các trọng số ngụ ý giống hệt nhau là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())

a, w = map(int, input().split())

if w % a != 0:
    print("DEFECT")
    sys.exit()

weight = w // a

for _ in range(k - 1):
    a, w = map(int, input().split())
    if w % a != 0 or w // a != weight:
        print("DEFECT")
        sys.exit()

print("QUALITY")
```Phép đo đầu tiên thiết lập trọng số nguyên duy nhất có thể. Trước khi lưu trữ nó, mã sẽ xác minh rằng phép chia là chính xác. Điều này ngăn cản việc chấp nhận trọng số phân số. 

Mỗi phép đo sau đó thực hiện kiểm tra khả năng chia hết tương tự trước khi so sánh trọng số ngụ ý với ứng cử viên được lưu trữ. Chỉ sử dụng phép chia số nguyên sau khi xác nhận tính chia hết để tránh việc cắt bớt không chính xác. 

Chương trình thoát ngay sau khi phát hiện bất kỳ mâu thuẫn nào. Vì một phép đo không nhất quán cũng đủ để loại bỏ lô nên không có lý do gì để xử lý dữ liệu đầu vào còn lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
10 30
5 15
2 6
```| Đo lường | một | w | Chia được | Trọng lượng ngụ ý | Ứng viên | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 30 | Có | 3 | 3 | Tiếp tục | 
| 2 | 5 | 15 | Có | 3 | 3 | Tiếp tục | 
| 3 | 2 | 6 | Có | 3 | 3 | Tiếp tục | 

Mỗi phép đo đều ngụ ý có cùng trọng số nguyên, vì vậy đầu ra là```
QUALITY
```Dấu vết này xác nhận sự bất biến rằng trọng số ứng cử viên vẫn nhất quán trong suốt quá trình quét. 

### Mẫu 2 

đầu vào:```
2
2 5
4 10
```| Đo lường | một | w | Chia được | Trọng lượng ngụ ý | Ứng viên | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 5 | Không | Không hợp lệ | Không có | Từ chối | 

Thuật toán dừng ngay lập tức vì phép đo đầu tiên không thể tương ứng với trọng số phần nguyên. 

Đầu ra là```
DEFECT
```Điều này chứng tỏ rằng trọng số phân số thông thường là không thể chấp nhận được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi phép đo được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Từ`k`tối đa là 1000, một đường truyền tuyến tính đơn giản đủ nhanh và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    k = int(input())
    a, w = map(int, input().split())

    if w % a != 0:
        print("DEFECT")
        return

    weight = w // a

    for _ in range(k - 1):
        a, w = map(int, input().split())
        if w % a != 0 or w // a != weight:
            print("DEFECT")
            return

    print("QUALITY")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples
assert run("3\n10 30\n5 15\n2 6\n") == "QUALITY", "sample 1"
assert run("2\n2 5\n4 10\n") == "DEFECT", "sample 2"

# custom cases
assert run("1\n7 35\n") == "QUALITY", "single valid measurement"
assert run("2\n2 6\n3 12\n") == "DEFECT", "different integer weights"
assert run("4\n1 9\n2 18\n3 27\n4 36\n") == "QUALITY", "all equal weight"
assert run("2\n1000 999000\n1 999\n") == "QUALITY", "boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một phép đo hợp lệ | CHẤT LƯỢNG | Kích thước đầu vào tối thiểu | 
| Hai trọng số nguyên ngụ ý khác nhau | LỖI | Phát hiện các phép đo không nhất quán | 
| Bốn phép đo nhất quán | CHẤT LƯỢNG | Nhiều bản ghi phù hợp | 
| Lớn`a`với tỷ lệ phù hợp | CHẤT LƯỢNG | Số học biên | 

## Vỏ cạnh 

Hãy xem xét trường hợp mỗi phép đo riêng lẻ đều cho một trọng số nguyên, nhưng các trọng số khác nhau.```
2
2 6
3 12
```Thuật toán lưu trữ trọng số ứng viên`3`từ lần đo đầu tiên. Phép đo thứ hai ngụ ý`4`, không phù hợp với ứng cử viên, vì vậy nó sẽ in`DEFECT`. Điều này nắm bắt được những mâu thuẫn mà phép kiểm tra tính chia hết đơn giản sẽ bỏ sót. 

Bây giờ hãy xem xét trọng số ngụ ý phân số.```
2
2 5
4 10
```Phép đo đầu tiên thất bại trong việc kiểm tra tính chia hết vì`5 % 2 != 0`. Thuật toán in ngay lập tức`DEFECT`mà không kiểm tra các hồ sơ sau này. Điều này từ chối chính xác các lô chỉ có thể được giải thích bằng trọng số không nguyên. 

Cuối cùng, hãy xem xét đầu vào hợp lệ nhỏ nhất.```
1
7 35
```Phép đo ngụ ý trọng lượng nguyên`5`. Không có bản ghi nào khác tồn tại mâu thuẫn với nó nên thuật toán kết thúc quá trình quét và in`QUALITY`. Điều này phù hợp với yêu cầu rằng bộ điều khiển chỉ từ chối lô khi bằng chứng thu thập được không nhất quán với mọi trọng số nguyên có thể có.
