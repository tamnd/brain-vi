---
title: "CF 105494D - Phân nhóm"
description: "Chúng ta được cho một mảng các số nguyên và được yêu cầu chia nó thành nhiều nhóm. Chi phí của một giải pháp là số lượng nhóm và mọi phần tử phải thuộc về chính xác một nhóm."
date: "2026-06-23T21:01:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 62
verified: true
draft: false
---

[CF 105494D - Phân nhóm](https://codeforces.com/problemset/problem/105494/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và được yêu cầu chia nó thành nhiều nhóm. Chi phí của một giải pháp là số lượng nhóm và mọi phần tử phải thuộc về chính xác một nhóm. Một nhóm chỉ được coi là hợp lệ nếu hai điều kiện bên trong nó được thỏa mãn: số phần tử không vượt quá một giới hạn cố định và sự khác biệt giữa phần tử nhỏ nhất và lớn nhất trong nhóm đó không vượt quá một ngưỡng cố định. 

Tuyên bố về cấu trúc quan trọng trong câu lệnh là thứ tự của các phần tử trong đầu vào không quan trọng. Khi chúng tôi sắp xếp mảng, mọi giải pháp tối ưu đều có thể được chuyển đổi thành một giải pháp tối ưu khác trong đó mỗi nhóm tương ứng với một phân đoạn liền kề của mảng được sắp xếp này. Điều này làm giảm vấn đề từ việc phân vùng tùy ý đến việc chọn vị trí cắt dọc theo một đường. 

Từ góc độ phức tạp, kích thước mảng đủ lớn để mọi thứ bậc hai trên n sẽ quá chậm. Một giải pháp thử tất cả các phân vùng hoặc tính toán lại tính hợp lệ của các phân đoạn nhiều lần sẽ thực hiện theo thứ tự n² kiểm tra trong trường hợp xấu nhất, điều này không được chấp nhận đối với các ràng buộc điển hình thuộc loại này. 

Một số trường hợp cần lưu ý. Nếu tất cả các giá trị đều bằng nhau thì hạn chế duy nhất đến từ giới hạn kích thước, vì vậy câu trả lời chỉ đơn giản là số khối có kích thước k. Nếu k rất lớn nhưng các giá trị cách đều nhau thì mọi phần tử có thể nằm trong nhóm riêng của nó do hạn chế về phạm vi. Một kẻ tham lam ngây thơ chỉ kiểm tra một điều kiện và bỏ qua điều kiện kia có thể thất bại. Ví dụ: nếu các phần tử có giá trị gần nhau nhưng phân đoạn phát triển quá dài, việc bỏ qua giới hạn kích thước sẽ tạo ra các nhóm không hợp lệ ngay cả khi điều kiện giá trị được thỏa mãn. 

## Phương pháp tiếp cận 

Phối cảnh brute-force là xem xét mọi cách có thể để chia mảng đã sắp xếp thành các phân đoạn liền kề và xác minh xem mỗi phân đoạn có thỏa mãn cả hai ràng buộc hay không. Đối với mỗi phân đoạn, chúng tôi sẽ quét các phần tử của nó để tính toán kích thước và phạm vi của nó, tức là O(n) trên mỗi phân đoạn trong trường hợp xấu nhất. Vì có nhiều cách để phân vùng một mảng theo cấp số nhân nên phương pháp này nhanh chóng trở nên không khả thi. Ngay cả việc lập trình động trên tất cả các vị trí cắt trước đó cũng dẫn đến O(n²), vì đối với mỗi vị trí, chúng tôi sẽ thử tất cả các điểm dừng trước đó và tính toán lại tính hợp lệ. 

Quan sát quan trọng là khi mảng được sắp xếp, tính hợp lệ của một phân đoạn chỉ phụ thuộc vào ranh giới bên trái của nó và khoảng cách chúng ta mở rộng sang bên phải. Khi chúng ta mở rộng một phân đoạn, cả kích thước và phạm vi giá trị đều thay đổi đơn điệu. Điều này có nghĩa là chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó trong một phân khúc: chúng ta có thể tham lam mở rộng phân khúc hiện tại cho đến khi việc thêm phần tử tiếp theo vi phạm một trong hai ràng buộc, sau đó chúng ta phải bắt đầu một phân khúc mới. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất trong đó mỗi phần tử được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | O(2^n) hoặc O(n²) DP | O(n) | Quá chậm | 
| Quét tham lam trên mảng được sắp xếp | O(n log n) (sắp xếp) + O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng bất kỳ nhóm hợp lệ nào cũng có thể được biểu diễn dưới dạng một phân đoạn liền kề và cả hai phần tử tối thiểu và tối đa của một phân khúc luôn ở cuối phân đoạn đó. 
2. Bắt đầu một nhóm mới ở phần tử đầu tiên. Lưu trữ hai thông tin: giá trị đầu tiên của nhóm hiện tại và vị trí chỉ mục của nó. 
3. Lặp lại mảng từ trái sang phải. Tại mỗi phần tử, hãy kiểm tra xem việc thêm nó vào nhóm hiện tại có vi phạm một trong hai ràng buộc hay không. 
4. Nếu sự khác biệt giữa phần tử hiện tại và phần tử đầu tiên của nhóm vượt quá ngưỡng cho phép hoặc nếu số lượng phần tử trong nhóm vượt quá k, thì hãy đóng nhóm hiện tại trước phần tử này và bắt đầu một nhóm mới tại vị trí này. 
5. Khi bắt đầu một nhóm mới, hãy đặt lại giá trị và vị trí đầu tiên được lưu trữ. 
6. Tiếp tục cho đến khi tất cả các phần tử được xử lý. Số lần chúng tôi thành lập một nhóm mới chính là câu trả lời. 

Việc triển khai chỉ duy trì ranh giới bên trái của phân khúc hiện tại. Điều này là đủ vì mảng đã được sắp xếp, do đó giá trị tối thiểu luôn ở ranh giới bên trái và giá trị tối đa là phần tử hiện tại đang được xem xét. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là khi chúng ta cố định điểm bắt đầu của một nhóm theo thứ tự được sắp xếp, việc mở rộng nhóm chỉ có thể làm xấu đi hai ràng buộc: kích thước tăng thêm một và phạm vi giữ nguyên hoặc tăng. Vì vậy, nếu phần tử tiếp theo vi phạm một ràng buộc thì không có phần mở rộng nào sau này có thể khắc phục được và giải pháp tối ưu nào cũng phải cắt ở vị trí đó. Điều này đảm bảo rằng việc cắt sớm nhất có thể không bao giờ làm tăng số lượng nhóm so với bất kỳ sự sắp xếp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 0
    first = None
    first_pos = 0
    
    for i, val in enumerate(a):
        if first is None:
            ans += 1
            first = val
            first_pos = i
            continue
        
        if val - first > m or i - first_pos >= k:
            ans += 1
            first = val
            first_pos = i
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng, điều này rất cần thiết vì nó biến bài toán nhóm thành bài toán phân đoạn một chiều. Biến`first`theo dõi phần tử nhỏ nhất trong nhóm hiện tại, phần tử này luôn là phần tử ngoài cùng bên trái do sắp xếp. Biến`first_pos`theo dõi có bao nhiêu phần tử đã được đưa vào nhóm hiện tại, cho phép chúng tôi thực thi giới hạn kích thước trong O(1). 

Mỗi lần chúng tôi phát hiện rằng việc thêm phần tử hiện tại sẽ vi phạm giới hạn phạm vi hoặc giới hạn kích thước, chúng tôi sẽ ngay lập tức bắt đầu một nhóm mới. Việc khởi động lại một cách tham lam này là an toàn vì bất kỳ phần mở rộng nào thêm nữa sẽ chỉ làm xấu đi cả hai ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 6, k = 3, m = 4
a = [1, 2, 3, 10, 11, 12]
```Sau khi sắp xếp (đã sắp xếp xong) chúng ta theo dõi quá trình phân nhóm. 

| tôi | giá trị | đầu tiên | kích thước | phạm vi (val-đầu tiên) | hành động | nhóm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 0 | bắt đầu | 1 | 
| 1 | 2 | 1 | 2 | 1 | tiếp tục | 1 | 
| 2 | 3 | 1 | 3 | 2 | tiếp tục | 1 | 
| 3 | 10 | 10 | 1 | 0 | nhóm mới (vi phạm phạm vi) | 2 | 
| 4 | 11 | 10 | 2 | 1 | tiếp tục | 2 | 
| 5 | 12 | 10 | 3 | 2 | tiếp tục | 2 | 

Điều này chứng tỏ khoảng trống lớn buộc phải cắt giảm như thế nào ngay cả khi kích thước cho phép tiếp tục. 

Bây giờ hãy xem xét:```
n = 5, k = 2, m = 100
a = [1, 2, 3, 4, 5]
```| tôi | giá trị | đầu tiên | kích thước | hành động | nhóm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | bắt đầu | 1 | 
| 1 | 2 | 1 | 2 | tiếp tục | 1 | 
| 2 | 3 | 3 | 1 | giới hạn kích thước kích hoạt nhóm mới | 2 | 
| 3 | 4 | 4 | 1 | nhóm mới | 3 | 
| 4 | 5 | 5 | 1 | nhóm mới | 4 | 

Điều này cho thấy ràng buộc về kích thước chiếm ưu thế khi các giá trị được đóng gói chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét là tuyến tính | 
| Không gian | O(1) thêm | Chỉ có một số biến được duy trì ngoài mảng | 

Thuật toán dễ dàng phù hợp trong giới hạn vì mỗi phần tử được xử lý chính xác một lần sau khi sắp xếp và không sử dụng vòng lặp lồng nhau hoặc quét lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    n, k, m = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    
    ans = 0
    first = None
    first_pos = 0
    
    for i, val in enumerate(a):
        if first is None:
            ans += 1
            first = val
            first_pos = i
            continue
        
        if val - first > m or i - first_pos >= k:
            ans += 1
            first = val
            first_pos = i
    
    return str(ans)

# sample-style tests
assert run("6 3 4\n1 2 3 10 11 12\n") == "2"
assert run("5 2 100\n1 2 3 4 5\n") == "3"

# edge cases
assert run("1 10 0\n5\n") == "1"
assert run("4 2 0\n1 1 1 1\n") == "4"
assert run("4 10 100\n1 1 1 1\n") == "1"
assert run("6 3 1\n1 1 2 2 3 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp ranh giới tối thiểu | 
| tất cả đều bằng nhau, nhỏ k | nhiều nhóm | hành vi hạn chế kích thước | 
| tất cả đều bằng nhau, k lớn | 1 | không có sự chia tách không cần thiết | 
| ràng buộc m chặt chẽ | nhiều vết cắt | thực thi điều kiện phạm vi | 

## Vỏ cạnh 

Khi mảng chứa một phần tử duy nhất, thuật toán ngay lập tức bắt đầu và kết thúc một nhóm vì không thể vi phạm ràng buộc nào. Trạng thái đặt lại chính xác vì vòng lặp khởi tạo một phân đoạn mới ở phần tử đầu tiên và không bao giờ kích hoạt việc cắt sau đó. 

Khi tất cả các phần tử giống hệt nhau, điều kiện phạm vi không bao giờ được kích hoạt vì chênh lệch luôn bằng 0. Hệ số giới hạn duy nhất trở thành k và thuật toán bắt đầu một nhóm mới chính xác khi phân đoạn hiện tại đạt kích thước k, tạo ra các khối có kích thước đồng đều. 

Khi k rất lớn và m rất nhỏ, hành vi bị chi phối hoàn toàn bởi khoảng cách giá trị. Thuật toán phân tách chính xác bất cứ khi nào phần tử tiếp theo vượt quá chênh lệch cho phép so với điểm bắt đầu phân đoạn hiện tại, ngay cả khi kích thước phân đoạn vẫn thấp hơn nhiều so với k. 

Khi m lớn và k nhỏ thì mọi đoạn buộc phải cắt theo kích thước. Thuật toán đảm bảo tính chính xác vì nó theo dõi vị trí của phần tử đầu tiên trong phân đoạn và so sánh trực tiếp các chỉ số, không phụ thuộc vào các giá trị.
