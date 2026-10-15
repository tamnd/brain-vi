---
title: "CF 105416B - Luộc chín"
description: "Chúng tôi được cung cấp một bộ sưu tập trứng, trong đó mỗi quả trứng cần một khoảng thời gian cụ thể để chín hoàn toàn. Jacob có tổng thời gian cố định và một chiếc nồi duy nhất, nghĩa là anh ấy chỉ có thể nấu một quả trứng mỗi lần mà không bị chồng chéo."
date: "2026-06-23T04:41:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105416
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 2 (Beginner)"
rating: 0
weight: 105416
solve_time_s: 96
verified: false
draft: false
---

[CF 105416B - Luộc chín](https://codeforces.com/problemset/problem/105416/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập trứng, trong đó mỗi quả trứng cần một khoảng thời gian cụ thể để chín hoàn toàn. Jacob có tổng thời gian cố định và một chiếc nồi duy nhất, nghĩa là anh ấy chỉ có thể nấu một quả trứng mỗi lần mà không bị chồng chéo. Khi bắt đầu nấu một quả trứng, anh ta phải dành toàn bộ thời gian nấu cho quả trứng đó trước khi chuyển sang quả trứng khác. 

Nhiệm vụ là xác định xem anh ta có thể nấu chín hoàn toàn bao nhiêu quả trứng nếu lên kế hoạch cho chúng theo cách tốt nhất có thể. Quyền tự do chính mà anh ta có là thứ tự nấu trứng và mục tiêu là tối đa hóa số lượng trứng hoàn thành trong tổng thời gian có sẵn. 

Hạn chế về số lượng trứng lên tới 100.000, trong khi giá trị thời gian và tổng thời gian có thể lên tới 2 tỷ. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các hoán vị hoặc mô phỏng rõ ràng tất cả các tập hợp con. Thậm chí một$O(n^2)$Cách tiếp cận này sẽ quá chậm nếu nó liên quan đến việc quét hoặc lựa chọn lặp đi lặp lại. 

Cạm bẫy tự nhiên xuất hiện khi người ta cho rằng lấy trứng theo thứ tự đầu vào là đủ. Ví dụ: nếu đầu vào là:```
t = 10
c = [9, 1, 1]
```Nấu theo thứ tự chỉ mang lại 1 quả trứng, vì 9 quả tiêu tốn gần như toàn bộ thời gian. Nhưng lựa chọn tối ưu nhất là lấy 2 quả trứng nhỏ trước, tổng cộng là 2 quả trứng. Điều này cho thấy việc đặt hàng là cần thiết. 

Một trường hợp khó phát hiện khác là khi tổng thời gian khớp chính xác với một sự kết hợp:```
t = 6
c = [4, 2, 2]
```Một chiến lược đúng đắn có thể nấu được cả ba, nhưng cách tiếp cận tham lam theo thứ tự ngây thơ có thể thất bại nếu chọn 4 trước. 

Vì vậy, vấn đề thực sự là chọn một tập hợp con thời gian nấu có tổng càng lớn càng tốt mà không vượt quá$t$, đồng thời tối đa hóa số lượng phần tử được chọn. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử từng tập hợp con trứng, tính tổng thời gian nấu và theo dõi tập hợp con lớn nhất phù hợp với thời gian. Điều này đúng vì nó đánh giá rõ ràng mọi khả năng. Tuy nhiên, số lượng tập hợp con tăng theo cấp số nhân khi$2^n$, và thậm chí đối với$n = 100000$, điều này hoàn toàn không thể thực hiện được. Thậm chí hạn chế kết hợp kích thước$k$không giúp ích gì, vì việc lặp lại tất cả các kết hợp vẫn phát nổ. 

Một quan sát có cấu trúc hơn xuất phát từ việc nhận thấy điều gì thực sự quyết định liệu một bộ trứng có khả thi hay không: chỉ tổng thời gian nấu mới quan trọng chứ không phải thứ tự của chúng. Vì mỗi quả trứng đóng góp tích cực vào tổng thời gian nên cách tốt nhất để lắp càng nhiều trứng càng tốt là giảm thiểu tổng chi phí cho mỗi kích cỡ bộ trứng đã chọn. Điều đó đương nhiên gợi ý nên chọn thời gian nấu nhỏ nhất trước tiên. 

Nếu chúng ta sắp xếp thời gian nấu theo thứ tự không giảm, thì việc lấy trứng từ nhỏ nhất trở lên sẽ đảm bảo rằng đối với bất kỳ tiền tố nào, chúng ta sẽ tối đa hóa số lượng trứng trong tổng thời gian nhỏ nhất có thể. Bất kỳ sự sai lệch nào so với thứ tự này sẽ thay thế chi phí nhỏ hơn hoặc bằng nhau bằng chi phí lớn hơn, điều này chỉ có thể làm giảm số lượng trứng phù hợp trong thời hạn. 

Sau khi sắp xếp, chúng tôi chỉ cần tích lũy số lần từ trái sang phải cho đến khi việc thêm một quả trứng khác vượt quá ngân sách. Độ dài tiền tố đạt được tại thời điểm đó là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^n)$|$O(n)$| Quá chậm | 
| Sắp xếp + tổng tiền tố tham lam |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng trứng$n$và tổng thời gian có sẵn$t$, sau đó đọc danh sách thời gian nấu. Điều này thiết lập các hạn chế về nguồn lực và chi phí cá nhân. 
2. Sắp xếp thời gian nấu theo thứ tự tăng dần. Bước này đảm bảo rằng chúng tôi luôn xem xét loại trứng rẻ nhất hiện có trước tiên, điều này cần thiết để tối đa hóa số lượng trứng có thể phù hợp với ngân sách. 
3. Khởi tạo hai biến: tổng hiện hành về tổng thời gian nấu đã sử dụng cho đến nay và bộ đếm số lượng trứng đã được nấu. 
4. Lặp lại danh sách thời gian nấu được sắp xếp từ nhỏ nhất đến lớn nhất. 
5. Đối với mỗi quả trứng, hãy kiểm tra xem việc thêm thời gian nấu có vượt quá tổng thời gian không.$t$. Nếu không, hãy đưa nó vào lịch trình bằng cách thêm thời gian của nó vào tổng chạy và tăng bộ đếm. 
6. Nếu việc thêm trứng hiện tại vượt quá giới hạn, hãy dừng lại ngay lập tức. Bất kỳ quả trứng nào nữa thậm chí còn lớn hơn hoặc bằng nhau, vì vậy chúng không thể được đưa vào mà không vi phạm ràng buộc. 
7. Xuất ra bộ đếm số lượng trứng tối đa có thể nấu được. 

Ý tưởng chính là chúng ta không bao giờ xem xét lại các quyết định trước đó vì việc phân loại đảm bảo rằng một khi quả trứng lớn hơn không thể vừa thì cũng không có quả trứng nào sau này có thể vừa. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là tất cả chi phí trứng đều độc lập và cộng gộp, đồng thời chúng tôi đang tối đa hóa số lượng theo một ràng buộc tổng duy nhất. Trong bất kỳ giải pháp tối ưu nào, nếu một quả trứng có chi phí lớn hơn được chọn trong khi vẫn còn một quả trứng chưa sử dụng có chi phí nhỏ hơn thì việc hoán đổi chúng không thể làm tăng tổng thời gian và không bao giờ làm giảm số lượng trứng. Việc áp dụng lặp đi lặp lại đối số hoán đổi này sẽ biến đổi bất kỳ giải pháp tối ưu nào thành giải pháp chọn tiền tố của danh sách đã sắp xếp. Tiền tố đó chính xác là cấu trúc của quá trình quét tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    c = list(map(int, input().split()))
    
    c.sort()
    
    total = 0
    count = 0
    
    for x in c:
        if total + x <= t:
            total += x
            count += 1
        else:
            break
    
    print(count)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc sắp xếp thời gian nấu sao cho chúng tôi luôn xử lý chi phí còn lại nhỏ nhất trước tiên. Đây là sự chuyển đổi cấu trúc làm cho cách tiếp cận tham lam trở nên hợp lý. Vòng lặp sau đó tích lũy thời gian cho đến khi ràng buộc bị vi phạm. Việc nghỉ sớm rất quan trọng vì khi chúng tôi vượt quá ngân sách, tất cả các giá trị tiếp theo ít nhất cũng lớn bằng quá trình sắp xếp. 

Một lỗi phổ biến là quên sắp xếp, điều này sẽ phá vỡ tính chính xác ngay lập tức vì giả định tham lam phụ thuộc hoàn toàn vào thứ tự tăng dần. Một vấn đề khác là không dừng sớm, điều này không cần thiết cho tính chính xác nhưng có thể làm giảm hiệu suất một chút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, t = 10
c = [9, 1, 1]
```Sau khi sắp xếp:```
[1, 1, 9]
```| Bước | Trứng | Tổng Chạy | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | lấy | 
| 2 | 1 | 2 | 2 | lấy | 
| 3 | 9 | 2 | 2 | dừng lại | 

Không thể thêm quả trứng thứ ba vì nó vượt quá ngân sách còn lại. Kết quả xác nhận rằng việc ưu tiên các giá trị nhỏ sẽ mang lại số lượng tối đa. 

### Ví dụ 2 

đầu vào:```
n = 5, t = 10
c = [2, 2, 2, 2, 2]
```Sau khi sắp xếp:```
[2, 2, 2, 2, 2]
```| Bước | Trứng | Tổng Chạy | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | lấy | 
| 2 | 2 | 4 | 2 | lấy | 
| 3 | 2 | 6 | 3 | lấy | 
| 4 | 2 | 8 | 4 | lấy | 
| 5 | 2 | 10 | 5 | lấy | 

Tất cả các quả trứng đều khớp chính xác, chứng tỏ rằng thuật toán xử lý chính xác các trường hợp đẳng thức chặt chẽ mà không dừng lại sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế, theo sau là một lần quét tuyến tính duy nhất | 
| Không gian |$O(1)$hoặc$O(n)$| Phụ thuộc vào việc triển khai sắp xếp tại chỗ | 

Các ràng buộc cho phép lên tới 100.000 quả trứng, vì vậy$O(n \log n)$Phương pháp sắp xếp phù hợp thoải mái trong giới hạn thời gian, trong khi quét tuyến tính vẫn có chi phí thấp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, t = map(int, input().split())
    c = list(map(int, input().split()))
    c.sort()
    total = 0
    count = 0
    for x in c:
        if total + x <= t:
            total += x
            count += 1
        else:
            break
    return str(count)

# provided samples
assert run("3 10\n9 1 1\n") == "2", "sample 1"
assert run("5 10\n2 2 2 2 2\n") == "5", "sample 2"

# custom cases
assert run("1 5\n10\n") == "0", "cannot cook any egg"
assert run("4 100\n1 2 3 4\n") == "4", "all fit easily"
assert run("4 5\n4 4 4 4\n") == "1", "only one fits"
assert run("6 7\n5 1 1 1 1 1\n") == "4", "prefix selection after sorting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5/10 | 0 | không có lựa chọn khả thi | 
| 4 100 / 1 2 3 4 | 4 | trường hợp tiêu thụ đầy đủ | 
| 4 5 / 4 4 4 4 | 1 | ranh giới ngân sách chặt chẽ | 
| 6 7 / 5 1 1 1 1 1 | 4 | sự đúng đắn của tiền tố tham lam | 

## Vỏ cạnh 

Một trường hợp nghiêm trọng là khi không thể nấu được quả trứng nào cả. Đối với đầu vào:```
n = 1, t = 3
c = [10]
```Sau khi sắp xếp, phần tử đầu tiên đã vượt quá ngân sách. Vòng lặp ngay lập tức bị ngắt mà không tăng bộ đếm, tạo ra 0. Điều này xác nhận rằng thuật toán xử lý chính xác hoàn toàn không khả thi. 

Một trường hợp đặc biệt khác là khi tất cả các quả trứng đều giống hệt nhau và khớp chính xác với ngân sách theo bội số. Vì:```
n = 3, t = 6
c = [2, 2, 2]
```Sắp xếp khiến danh sách không thay đổi. Thuật toán thêm từng quả trứng một cách tuần tự, đạt chính xác 6. Vì sự bình đẳng được cho phép nên nó bao gồm cả ba quả trứng. Điều này thể hiện việc xử lý đúng các điều kiện đẳng thức biên trong đó tổng khớp chính xác với giới hạn mà không bị tràn hoặc dừng sớm.
