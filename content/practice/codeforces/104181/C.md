---
title: "CF 104181C - Làm Bánh Brownie"
description: "Chúng ta được giao cho một nhóm bạn, mỗi người đều muốn một chiếc bánh hạnh nhân có kích thước tối thiểu nhất định. Chúng tôi cũng được tặng một bộ sưu tập khuôn nướng bánh, mỗi khuôn sản xuất chính xác một chiếc bánh hạnh nhân có kích thước cố định."
date: "2026-07-02T00:37:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 58
verified: true
draft: false
---

[CF 104181C - Nướng bánh hạnh nhân](https://codeforces.com/problemset/problem/104181/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm bạn, mỗi người đều muốn một chiếc bánh hạnh nhân có kích thước tối thiểu nhất định. Chúng tôi cũng được tặng một bộ sưu tập khuôn nướng bánh, mỗi khuôn sản xuất chính xác một chiếc bánh hạnh nhân có kích thước cố định. Mỗi hộp thiếc có thể được sử dụng tối đa một lần và mỗi người bạn có thể nhận được tối đa một bánh hạnh nhân. Một người bạn hài lòng nếu chiếc bánh hạnh nhân họ nhận được hoàn toàn lớn hơn kích cỡ họ yêu cầu. 

Nhiệm vụ là ghép các hộp thiếc với bạn bè sao cho tối đa hóa số lượng bạn bè hài lòng. 

Đầu vào bao gồm hai mảng. Phần đầu tiên mô tả kích thước bánh hạnh nhân tối thiểu được chấp nhận cho mỗi người bạn. Phần thứ hai mô tả các kích cỡ bánh hạnh nhân có sẵn do hộp thiếc sản xuất. Đầu ra là một số nguyên duy nhất: số lượng cặp hợp lệ tối đa trong đó kích thước hộp thiếc lớn hơn yêu cầu của bạn bè. 

Các ràng buộc lên tới 200.000 phần tử trong cả hai mảng. Bất kỳ giải pháp bậc hai nào trong trường hợp xấu nhất sẽ yêu cầu so sánh theo thứ tự 4 × 10^10, vượt xa những gì có thể được thực thi trong hai giây bằng Python. Điều này ngay lập tức loại trừ việc ghép đôi hoặc kiểm tra mọi nhiệm vụ có thể thực hiện được. 

Một điểm tinh tế của bài toán là điều kiện bất đẳng thức nghiêm ngặt. Một hộp thiếc có kích thước chính xác như yêu cầu không làm hài lòng người bạn. Điều này thay đổi cách chúng tôi tìm kiếm kết quả phù hợp vì sự bình đẳng không được chấp nhận và phải được bỏ qua cẩn thận trong quá trình kết hợp. 

Vấn đề tế nhị thứ hai là các bài tập tối ưu không rõ ràng ở địa phương. Đưa một hộp thiếc lớn cho một yêu cầu nhỏ đôi khi có thể cản trở một mối quan hệ tốt hơn trong tương lai, vì vậy những lựa chọn tham lam phải được biện minh một cách cẩn thận. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là xem xét mọi cách phân công hộp thiếc có thể có cho bạn bè và chọn hộp thiếc phù hợp nhất. Ngay cả việc hạn chế bản thân thử từng người bạn với mọi người cũng mang lại sự so sánh N × M. Với cả hai đều lên tới 2 × 10^5, điều này trở nên không khả thi. 

Một cách tiếp cận khác được cải tiến đôi chút nhưng vẫn quá chậm là sắp xếp cả hai mảng và đối với mỗi người bạn, hãy quét tiếp trong mảng tins để tìm tin hợp lệ đầu tiên. Ngay cả khi sắp xếp, quá trình quét lồng nhau vẫn chuyển sang trạng thái bậc hai khi nhiều hộp thiếc quá nhỏ vì mỗi hộp thiếc có thể được kiểm tra nhiều lần. 

Quan sát quan trọng là cả hai mảng đều là các tập hợp kích thước độc lập và chúng tôi chỉ quan tâm đến việc khớp chúng trong điều kiện đơn điệu. Sau khi được sắp xếp, cấu trúc sẽ trở nên tuyến tính: nếu chúng tôi xử lý yêu cầu nhỏ nhất chưa được đáp ứng và luôn cố gắng đáp ứng yêu cầu đó bằng hộp thiếc nhỏ nhất có thể mà vẫn hoạt động được, chúng tôi sẽ tránh lãng phí hộp thiếc lớn. 

Điều này tự nhiên dẫn đến một chiến lược tham lam: sắp xếp cả hai mảng và di chuyển qua chúng bằng hai con trỏ. Chúng tôi cố gắng ghép yêu cầu nhỏ nhất còn lại với hộp thiếc nhỏ nhất có thể đáp ứng được yêu cầu đó. Nếu hộp thiếc quá nhỏ, chúng ta sẽ vứt nó đi và đi tiếp, vì nó không thể giúp được bất kỳ yêu cầu nào lớn hơn mà chúng ta chưa đạt được. Nếu nó đủ lớn, chúng tôi khớp chúng và nâng cao cả hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi vũ phu | O(NM) | O(1) | Quá chậm | 
| Sắp xếp + tham lam hai con trỏ | O(N log N + M log M) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp cả yêu cầu kết bạn và kích cỡ hộp thiếc theo thứ tự tăng dần. Điều này sắp xếp vấn đề để chúng tôi luôn giải quyết nhu cầu chưa được đáp ứng nhỏ nhất còn lại trước tiên. 

Chúng tôi duy trì hai chỉ số, một chỉ số cho bạn bè và một chỉ số cho hộp thiếc, đồng thời chúng tôi cũng duy trì một bộ đếm cho các trận đấu thành công.

1. Sắp xếp mảng yêu cầu kết bạn theo thứ tự tăng dần và sắp xếp mảng kích thước thiếc theo thứ tự tăng dần. Điều này đảm bảo chúng ta có thể suy luận tham lam từ nhỏ nhất đến lớn nhất mà không bỏ lỡ các cặp tối ưu. 
2. Khởi tạo hai con trỏ, i cho bạn bè và j cho tins, cả hai đều bắt đầu từ 0. Đồng thời khởi tạo một biến có giá trị bằng 0. Con trỏ i đại diện cho yêu cầu nhỏ nhất chưa được đáp ứng và j đại diện cho tin nhỏ nhất chưa được sử dụng. 
3. Trong khi cả hai con trỏ đều nằm trong giới hạn, hãy so sánh s[i] và t[j]. Nếu t[j] lớn hơn s[i], chúng ta gán tin này cho người bạn này, tăng dần bằng 1 và nâng cao cả hai con trỏ. Điều này là an toàn vì t[j] là hộp thiếc nhỏ nhất có thể thỏa mãn s[i], vì vậy bất kỳ lựa chọn nào khác sẽ chỉ lãng phí hộp thiếc lớn hơn. 
4. Nếu t[j] nhỏ hơn hoặc bằng s[i], chúng ta loại bỏ tin này bằng cách tăng j. Nó không thể làm hài lòng người bạn hiện tại, và vì tất cả những người bạn tương lai đều có những yêu cầu tương đương hoặc lớn hơn nên nó cũng không thể làm hài lòng họ, nên nó vĩnh viễn vô dụng. 
5. Tiếp tục cho đến khi hết một trong hai mảng. Giá trị phù hợp là số lượng bạn bè hài lòng tối đa. 

Tại sao nó hoạt động phụ thuộc vào một đối số thống trị đối với các mảng được sắp xếp. Ở bất kỳ bước nào, việc ghép nối người bạn không hài lòng nhỏ nhất với hộp thiếc nhỏ nhất có thể sử dụng được sẽ duy trì tính linh hoạt trong tương lai. Thay vào đó, nếu chúng tôi sử dụng hộp thiếc lớn hơn, chúng tôi sẽ chỉ giảm số lượng tùy chọn có sẵn cho các yêu cầu lớn hơn sau này mà không cải thiện kết quả phù hợp hiện tại. Thuật toán duy trì tính bất biến rằng tất cả các hộp thiếc trước j đều đã được sử dụng hoặc quá nhỏ để có thể sử dụng được và tất cả bạn bè trước tôi đều đã hài lòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    t = list(map(int, input().split()))
    
    s.sort()
    t.sort()
    
    i = j = 0
    matched = 0
    
    while i < n and j < m:
        if t[j] > s[i]:
            matched += 1
            i += 1
            j += 1
        else:
            j += 1
    
    print(matched)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là quét hai con trỏ sau khi sắp xếp. Việc sắp xếp là cần thiết vì quyết định tham lam chỉ đúng khi cả hai chuỗi đều đơn điệu. Nếu không sắp xếp, các quyết định cục bộ không có sự đảm bảo toàn cầu. 

Bất đẳng thức chặt chẽ được xử lý trực tiếp trong so sánh`t[j] > s[i]`. sử dụng`>=`ở đây sẽ tính không chính xác các hộp thiếc có kích thước bằng nhau là hợp lệ, vi phạm điều kiện của vấn đề. 

Con trỏ tiến lên một cách có kiểm soát: tôi chỉ di chuyển khi bạn bè hài lòng, j luôn tiến về phía trước và không bao giờ xem lại hộp thiếc, đảm bảo quét tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
8 12 25 3 10
1 8 20
```Đã sắp xếp: 

s = [3, 8, 10, 12, 25] 

t = [1, 8, 20] 

| tôi | j | s[i] | t[j] | Hành động | khớp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 1 | 1 ≤ 3, loại bỏ thiếc | 0 | 
| 0 | 1 | 3 | 8 | 8 > 3, khớp | 1 | 
| 1 | 2 | 8 | 20 | 20 > 8, trận đấu | 2 | 

Đầu ra là 2. 

Dấu vết này cho thấy rằng các hộp thiếc nhỏ được loại bỏ một cách an toàn ngay cả khi chúng có vẻ có thể sử dụng được sau này, bởi vì có những yêu cầu lớn hơn nhưng vẫn đảm bảo tính khả thi. 

### Ví dụ tùy chỉnh 

đầu vào:```
4 4
5 6 7 8
4 5 6 10
```Đã sắp xếp: 

s = [5, 6, 7, 8] 

t = [4, 5, 6, 10] 

| tôi | j | s[i] | t[j] | Hành động | khớp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 4 | vứt bỏ | 0 | 
| 0 | 1 | 5 | 5 | loại bỏ (không hoàn toàn lớn hơn) | 0 | 
| 0 | 2 | 5 | 6 | trận đấu | 1 | 
| 1 | 3 | 6 | 10 | trận đấu | 2 | 

Điều này thể hiện rõ ràng hành vi bất bình đẳng nghiêm ngặt, trong đó sự bình đẳng vẫn đòi hỏi phải bỏ qua hộp thiếc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + M log M) | Sắp xếp cả hai mảng chiếm ưu thế, quét hai con trỏ là tuyến tính | 
| Không gian | O(1) thêm | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 2 × 10^5 phần tử, do đó, việc sắp xếp ở tỷ lệ này nằm trong giới hạn trong Python và quá trình quét tuyến tính sẽ bổ sung thêm chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    t = list(map(int, input().split()))
    s.sort()
    t.sort()
    i = j = 0
    matched = 0
    while i < n and j < m:
        if t[j] > s[i]:
            matched += 1
            i += 1
            j += 1
        else:
            j += 1
    print(matched)

# provided sample
assert run("""5 3
8 12 25 3 10
1 8 20
""") == "2"

# all tins too small
assert run("""3 3
10 20 30
1 2 3
""") == "0"

# all tins large enough
assert run("""3 3
1 2 3
10 10 10
""") == "3"

# equality edge case
assert run("""3 3
5 5 5
5 6 7
""") == "2"

# mixed case
assert run("""5 4
2 4 6 8 10
1 3 5 9
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các hộp thiếc quá nhỏ | 0 | không có trận đấu ngẫu nhiên | 
| tất cả các hộp thiếc lớn | 3 | có thể kết hợp đầy đủ | 
| trường hợp bình đẳng | 2 | xử lý bất bình đẳng nghiêm ngặt | 
| trường hợp hỗn hợp | 3 | tham lam ghép nối đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tranh nhất là khi có nhiều hộp thiếc hoàn toàn bằng yêu cầu. Ví dụ: 

đầu vào:```
3 3
5 5 5
5 6 7
```Sau khi sắp xếp, thuật toán sẽ so sánh 5 với 5 trước tiên. Vì điều kiện nghiêm ngặt nên nó sẽ loại bỏ thiếc. Việc này lặp lại cho đến khi đạt đến con số 6, chỉ tạo ra hai trận đấu. Việc triển khai có lỗi sử dụng`>=`thay vì`>`sẽ đếm sai cặp đầu tiên và trả về 3, vi phạm ràng buộc bài toán. 

Một trường hợp cạnh khác xảy ra khi tất cả các hộp thiếc đều nhỏ hơn tất cả các yêu cầu:```
3 3
10 20 30
1 2 3
```Con trỏ trên hộp thiếc tiến lên cho đến khi cạn kiệt mà không có bất kỳ kết quả trùng khớp nào. Thuật toán trả về chính xác 0 vì không có tin nào thỏa mãn điều kiện bất đẳng thức nghiêm ngặt và mọi loại bỏ đều được chứng minh bằng tính đơn điệu sau khi sắp xếp.
