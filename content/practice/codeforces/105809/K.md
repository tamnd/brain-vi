---
title: "CF 105809K - Tối ưu hóa ngôn ngữ K-token"
description: "Chúng ta được cho một mảng có độ dài n và giới hạn k. Chúng tôi muốn chia mảng thành số đoạn liền kề nhỏ nhất có thể. Bên trong mỗi đoạn, số lượng giá trị phân biệt không được vượt quá k. Nhiệm vụ không phải là đếm tất cả các phân vùng hợp lệ."
date: "2026-06-25T15:30:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "K"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 40
verified: true
draft: false
---

[CF 105809K - Tối ưu hóa ngôn ngữ K-token](https://codeforces.com/problemset/problem/105809/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài`n`và một giới hạn`k`. Chúng tôi muốn chia mảng thành số đoạn liền kề nhỏ nhất có thể. Trong mỗi đoạn, số lượng giá trị phân biệt không được vượt quá`k`. 

Nhiệm vụ không phải là đếm tất cả các phân vùng hợp lệ. Chúng tôi chỉ cần số lượng phân đoạn tối thiểu cần thiết để bao phủ toàn bộ mảng trong khi vẫn tôn trọng giới hạn giá trị riêng biệt. 

Những hạn chế khá rõ ràng. Độ dài mảng có thể đạt tới`10^5`, do đó, bất kỳ thuật toán nào kiểm tra tất cả các mảng con hoặc thực hiện lập trình động trên tất cả các khoảng đều quá tốn kém. MỘT`O(n²)`giải pháp sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Một quan sát hữu ích khác đến từ phạm vi giá trị. Mọi phần tử mảng đều thỏa mãn`1 ≤ a[i] ≤ 1000`, Và`k ≤ 1000`. Vì các giá trị nhỏ nên việc duy trì tần số của các phần tử bên trong cửa sổ trượt là rất rẻ. 

Một sai lầm phổ biến là nghĩ rằng việc cân bằng độ dài các đoạn có thể hữu ích. Mục tiêu chỉ là giảm thiểu số lượng phân đoạn. Bản thân kích thước phân khúc không liên quan. 

Hãy xem xét mảng:```
3 1
1 2 3
```Mỗi phân đoạn có thể chứa nhiều nhất một giá trị riêng biệt. Phân vùng hợp lệ duy nhất là:```
[1] [2] [3]
```Câu trả lời là:```
3
```Một trường hợp dễ bỏ sót khác là:```
5 2
1 2 1 2 3
```Một chiến lược bất cẩn có thể bị cắt bỏ ngay khi phân khúc đã chứa`k`những giá trị riêng biệt. Điều đó sẽ tạo ra:```
[1 2 1 2] [3]
```điều này thực sự tối ưu và đưa ra câu trả lời`2`. 

Chi tiết quan trọng là một phân đoạn có thể chứa chính xác`k`những giá trị riêng biệt. Chúng ta chỉ cần cắt khi thêm phần tử tiếp theo sẽ tăng số lượng vượt quá`k`. 

Trường hợp cạnh thứ ba là khi tất cả các giá trị giống hệt nhau:```
3 1
1 1 1
```Toàn bộ mảng đã thỏa mãn điều kiện nên câu trả lời là:```
1
```Bất kỳ hoạt động triển khai nào tạo ra một phân khúc mới một cách mù quáng bất cứ khi nào nó gặp phải ranh giới lặp lại sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là xây dựng các phân đoạn từ trái sang phải và đối với mỗi vị trí bắt đầu, hãy thử tất cả các vị trí kết thúc có thể có trong khi đếm các giá trị riêng biệt. Khi một phân đoạn được chọn, hãy lặp lại hậu tố còn lại. 

Điều này hoạt động vì mọi phân vùng hợp lệ đều có thể được khám phá. Vấn đề là số lượng khoảng thời gian. có`O(n²)`các mảng con và thậm chí kiểm tra số lượng riêng biệt một cách hiệu quả cũng không giải quyết được độ phức tạp tổng thể. Với`n = 10^5`, cách tiếp cận như vậy là vô vọng. 

Quan sát chính là chúng ta chỉ cần số lượng phân đoạn tối thiểu và mỗi phân đoạn bị ràng buộc bởi số lượng giá trị riêng biệt tối đa. 

Giả sử chúng ta hiện đang xây dựng một phân khúc. Miễn là việc thêm phần tử tiếp theo sẽ giữ tối đa số lượng giá trị riêng biệt`k`, việc mở rộng phân khúc không bao giờ có thể gây tổn hại. Việc tạo một đoạn dài hơn chỉ để lại ít phần tử hơn cho các đoạn sau. Vì mục tiêu của chúng ta là giảm thiểu số lượng phân đoạn nên chúng ta phải luôn mở rộng phân khúc hiện tại càng nhiều càng tốt. 

Điều này ngay lập tức gợi ý một chiến lược tham lam. Bắt đầu một phân đoạn, tiếp tục thêm các phần tử trong khi phân đoạn đó chứa nhiều nhất`k`các giá trị riêng biệt và thời điểm phần tử tiếp theo sẽ tạo ra`(k+1)`-giá trị riêng biệt thứ , đóng phân đoạn hiện tại và bắt đầu một phân đoạn mới từ phần tử đó. 

Tại sao sự lựa chọn tham lam này là tối ưu? Bất kỳ phân vùng hợp lệ nào cũng phải kết thúc phân đoạn hiện tại không muộn hơn thời điểm mà phân vùng đó`(k+1)`-giá trị riêng biệt thứ xuất hiện. Phân đoạn tham lam của chúng tôi là phân đoạn hợp lệ dài nhất. Việc thay thế bất kỳ phân đoạn đầu tiên ngắn hơn nào bằng phân đoạn hợp lệ dài nhất này không thể làm tăng số lượng phân đoạn cần thiết sau này. Việc lặp lại cùng một đối số cho hậu tố còn lại chứng tỏ tính tối ưu. 

Bởi vì giá trị tối đa là`1000`, chúng ta có thể duy trì tần số và số lượng giá trị riêng biệt hiện tại trong thời gian không đổi trên mỗi phần tử. Mỗi phần tử được xử lý chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Các giá trị riêng biệt đếm tham lam tối ưu | O(n) | O(1000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng tần số cho các giá trị`1..1000`. 
2. Bắt đầu với một phân đoạn và cấu trúc tần số trống. 
3. Duyệt mảng từ trái sang phải. 
4. Đối với giá trị hiện tại`x`, kiểm tra xem nó đã tồn tại trong phân khúc hiện tại chưa. 
5. Nếu`x`là mới và phân đoạn hiện tại đã chứa chính xác`k`các giá trị riêng biệt, sau đó thêm`x`sẽ vi phạm ràng buộc. Bắt đầu một phân khúc mới. 

Đặt lại tất cả các tần số, đặt lại bộ đếm riêng biệt về 0 và tăng câu trả lời lên một. 
6. Chèn`x`vào phân đoạn hiện tại bằng cách cập nhật tần số của nó. 
7. Nếu đây là lần đầu tiên xảy ra`x`trong phân khúc hiện tại, tăng bộ đếm riêng biệt. 
8. Sau khi xử lý tất cả các phần tử, xuất ra số đoạn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, phân đoạn hiện tại được thuật toán duy trì là phân đoạn hợp lệ dài nhất kết thúc ở vị trí hiện tại. Bất cứ khi nào một giá trị khác biệt mới làm cho số lượng vượt quá`k`, mọi phân vùng hợp lệ buộc phải kết thúc phân đoạn hiện tại trước giá trị đó. Sự lựa chọn tham lam trì hoãn việc cắt giảm càng nhiều càng tốt, tạo ra phân khúc khả thi lớn nhất. Phân đoạn đầu tiên lớn hơn không bao giờ có thể yêu cầu nhiều phân đoạn cho hậu tố còn lại hơn phân đoạn đầu tiên nhỏ hơn. Việc áp dụng đối số này nhiều lần sẽ mang lại một phân vùng có số lượng phân đoạn tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

freq = [0] * 1001
distinct = 0
ans = 1

for x in a:
    if freq[x] == 0 and distinct == k:
        ans += 1
        freq = [0] * 1001
        distinct = 0

    if freq[x] == 0:
        distinct += 1
    freq[x] += 1

print(ans)
```Việc thực hiện tuân theo chiến lược tham lam một cách chính xác. 

Biến`distinct`lưu trữ số lượng các giá trị khác nhau hiện có trong phân đoạn hoạt động. Mảng tần số cho phép chúng tôi xác định theo thời gian không đổi xem giá trị có xuất hiện lần đầu tiên trong phân đoạn đó hay không. 

Phần tinh tế là thứ tự thực hiện các thao tác. Trước tiên chúng ta phải kiểm tra xem việc thêm giá trị hiện tại có vượt quá giới hạn giá trị riêng biệt hay không. Chỉ sau khi quyết định xem có cần phân đoạn mới hay không, chúng tôi mới chèn giá trị. 

Khi một phân đoạn mới bắt đầu, phần tử hiện tại thuộc về phân đoạn mới đó. Đây là lý do tại sao logic chèn chạy sau khi thiết lập lại. 

Phạm vi giá trị được giới hạn bởi`1000`, do đó việc đặt lại mảng tần số chỉ tốn chi phí`O(1000)`thời gian. Vì nhiều nhất`n`các phân đoạn có thể tồn tại và`1000`là một hằng số, độ phức tạp tổng thể vẫn tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 3
```| Vị trí | Giá trị | Khác Biệt Trước | Hành động | Phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | Thêm | 1 | 
| 2 | 2 | 1 | Thêm | 1 | 
| 3 | 3 | 2 | Cần phân khúc mới | 2 | 

Đầu ra:```
2
```Đoạn đầu tiên trở thành`[1, 2]`. Thêm`3`sẽ tạo ra giá trị riêng biệt thứ ba, do đó một phân khúc mới sẽ bắt đầu. 

### Ví dụ 2 

đầu vào:```
5 2
1 2 1 2 3
```| Vị trí | Giá trị | Khác Biệt Trước | Hành động | Phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | Thêm | 1 | 
| 2 | 2 | 1 | Thêm | 1 | 
| 3 | 1 | 2 | Thêm | 1 | 
| 4 | 2 | 2 | Thêm | 1 | 
| 5 | 3 | 2 | Cần phân khúc mới | 2 | 

Đầu ra:```
2
```Dấu vết này cho thấy lý do tại sao chúng ta nên tiếp tục mở rộng phân khúc trong khi số lượng riêng biệt vẫn nằm trong giới hạn. Bốn yếu tố đầu tiên phù hợp trong một phân khúc duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử mảng được xử lý một lần | 
| Không gian | O(1000) | Mảng tần số cho các giá trị 1..1000 | 

Từ`n`nhiều nhất là`10^5`, một nghiệm thời gian tuyến tính dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ là không đổi đối với`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * 1001
    distinct = 0
    ans = 1

    for x in a:
        if freq[x] == 0 and distinct == k:
            ans += 1
            freq = [0] * 1001
            distinct = 0

        if freq[x] == 0:
            distinct += 1
        freq[x] += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided samples
assert run("3 1\n1 2 3\n") == "3"
assert run("3 2\n1 2 3\n") == "2"
assert run("3 1\n1 1 1\n") == "1"

# custom cases
assert run("1 1\n7\n") == "1"
assert run("5 5\n1 2 3 4 5\n") == "1"
assert run("6 2\n1 2 1 2 1 2\n") == "1"
assert run("6 2\n1 2 3 1 2 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`1`| Đầu vào kích thước tối thiểu | 
|`5 5 / 1 2 3 4 5`|`1`| Số lượng riêng biệt chính xác bằng k | 
|`6 2 / 1 2 1 2 1 2`|`1`| Giá trị lặp lại không tạo phân đoạn mới | 
|`6 2 / 1 2 3 1 2 3`|`3`| Nhiều lần cắt buộc | 

## Vỏ cạnh 

Hãy xem xét:```
3 1
1 2 3
```Thuật toán bắt đầu với số lượng phân đoạn`1`. Sau khi đọc`1`, phân đoạn có một giá trị riêng biệt. Đọc`2`sẽ tạo ra một giá trị khác biệt thứ hai trong khi`k = 1`, vì vậy một phân đoạn mới bắt đầu. Điều tương tự cũng xảy ra đối với`3`. Câu trả lời cuối cùng là`3`, điều này là tối ưu vì mỗi phân đoạn chỉ có thể chứa một giá trị riêng biệt. 

Bây giờ hãy xem xét:```
5 2
1 2 1 2 3
```Sau khi xử lý bốn phần tử đầu tiên, đoạn này chứa chính xác hai giá trị riêng biệt,`{1,2}`. Khi`3`xuất hiện, việc thêm nó sẽ tạo ra giá trị khác biệt thứ ba. Thuật toán bắt đầu một đoạn mới và đặt`3`ở đó. Câu trả lời trở thành`2`. Bất kỳ phân vùng nào có một phân đoạn đều không hợp lệ vì toàn bộ mảng chứa ba giá trị riêng biệt. 

Cuối cùng:```
3 1
1 1 1
```Số lượng riêng biệt không bao giờ vượt quá một. Không cần phải cắt và thuật toán đưa ra`1`. Điều này xác nhận rằng sự xuất hiện lặp đi lặp lại của một giá trị đã có sẵn không ảnh hưởng đến giới hạn giá trị khác biệt.
