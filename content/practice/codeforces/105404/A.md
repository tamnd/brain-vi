---
title: "CF 105404A - Không còn ràng buộc nữa!"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, một cuộc thi có một danh sách điểm của người tham gia và chỉ k người tham gia hàng đầu mới đủ điều kiện."
date: "2026-06-23T17:16:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105404
codeforces_index: "A"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105404
solve_time_s: 70
verified: true
draft: false
---

[CF 105404A - Không còn ràng buộc nữa!](https://codeforces.com/problemset/problem/105404/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, một cuộc thi có một danh sách điểm của người tham gia và chỉ có điểm cao nhất mới được`k`người tham gia được cho là đủ điều kiện. Vấn đề là trình độ chuyên môn hoàn toàn phụ thuộc vào xếp hạng điểm số, nhưng sự ràng buộc về điểm số có thể khiến việc quyết định chính xác ai là người đứng đầu là không thể`k`mà không cần xem xét các quy tắc hòa bổ sung như thời gian phạt đền. 

Đối với mỗi trường hợp kiểm thử, chúng ta phải xác định xem liệu điểm giới hạn giữa điểm cao nhất có`k`và phần còn lại rõ ràng hoặc mơ hồ. Sự tách biệt rõ ràng có nghĩa là tất cả những người tham gia ở cấp cao nhất`k`các vị trí có điểm số cao hơn hẳn so với những người bên ngoài. Sự phân tách không rõ ràng xảy ra khi ranh giới vượt qua một nhóm có điểm bằng nhau, nghĩa là một số người tham gia ở ranh giới có điểm giống hệt nhau và không thể phân chia duy nhất thành “đủ tiêu chuẩn” và “không đủ tiêu chuẩn” chỉ bằng cách sử dụng điểm số. 

Đầu ra là một quyết định nhị phân cho mỗi trường hợp thử nghiệm. Chúng tôi in ra câu trả lời tích cực khi phân vùng sạch sẽ và cảnh báo hòa khi ranh giới nằm trong nhóm có điểm bằng nhau. 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm và tối đa 10.000 người tham gia mỗi trường hợp. Một giải pháp sắp xếp từng trường hợp thử nghiệm một cách độc lập sẽ đủ nhanh vì việc sắp xếp 10.000 phần tử tốn khoảng`O(n log n)`nằm trong giới hạn ngay cả đối với 100 trường hợp. Bất kỳ cách tiếp cận nào cố gắng so sánh từng cặp người tham gia đều không cần thiết nhưng vẫn có khả năng vượt qua ở quy mô này; tuy nhiên, về mặt khái niệm thì nó quá chậm và che khuất cấu trúc của vấn đề. 

Trường hợp góc tinh tế xuất hiện khi`k`- Điểm thứ đó bằng với`(k+1)`-điểm thứ. Trong tình huống đó, không có cách nào để quyết định ai là người đứng đầu`k`không có quy luật bên ngoài. Một trường hợp khác là khi tất cả các điểm đều giống nhau, trong đó mọi giới hạn có thể đều không rõ ràng bất kể`k`. 

Ví dụ về sự mơ hồ: 

đầu vào:`n = 5, k = 3, scores = [10, 10, 10, 5, 1]`Ở đây, ba con số đứng đầu đều là số 10, nhưng bất kỳ thứ tự nào trong số chúng đều ổn và điểm cắt rõ ràng, vì vậy điều này thực sự ổn. 

Ví dụ về vấn đề ràng buộc: 

đầu vào:`n = 5, k = 2, scores = [10, 10, 10, 5, 1]`Vị trí thứ hai và thứ ba đều có điểm 10 nên việc chọn ra chính xác hai trong số ba thí sinh bị hòa là điều không thể nếu không có người hòa. Đầu ra phải chỉ ra sự mơ hồ. 

Câu hỏi cốt lõi giảm xuống việc kiểm tra xem ranh giới giữa chỉ số`k`Và`k+1`theo thứ tự sắp xếp vượt qua các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là mô phỏng quá trình xếp hạng. Người ta có thể sắp xếp những người tham gia theo điểm số theo thứ tự giảm dần và sau đó kiểm tra vị trí giới hạn. Sau khi sắp xếp, chúng ta kiểm tra điểm ở vị trí`k`và so sánh nó với điểm số ở vị trí`k+1`. Nếu chúng khác nhau thì ranh giới rõ ràng. Nếu chúng bằng nhau thì ranh giới nằm trong một nhóm ràng buộc và sự phân chia không rõ ràng. 

Một biến thể bạo lực có thể thử, đối với mỗi người tham gia, để xác định xem họ có thể là một phần của một số giải pháp hàng đầu hợp lệ hay không.`k`phân công theo các ràng buộc ràng buộc, nhưng điều này nhanh chóng trở nên phức tạp không cần thiết. Cấu trúc của vấn đề đảm bảo rằng chỉ có mối quan hệ giữa`k`-th và`(k+1)`-th giá trị lớn nhất quan trọng. Mọi thứ khác đều không liên quan vì việc sắp xếp bên trong các nhóm có giá trị bằng nhau không làm thay đổi tính khả thi. 

Thông tin chi tiết quan trọng là chỉ có ranh giới giới hạn mới quan trọng chứ không phải xếp hạng đầy đủ. Sau khi điểm được sắp xếp, toàn bộ quyết định sẽ giảm xuống thành một so sánh duy nhất giữa hai phần tử liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ các bài tập xếp hạng | O(n²) | O(n) | Quá chậm | 
| Sắp xếp và kiểm tra ranh giới | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm được xử lý độc lập vì không có trạng thái nào được chia sẻ. 
2. Với mỗi test case, hãy đọc`n`Và`k`, sau đó đọc danh sách điểm. Thứ tự đầu vào không liên quan vì việc xếp hạng chỉ phụ thuộc vào việc sắp xếp. 
3. Sắp xếp điểm theo thứ tự giảm dần. Điều này tạo ra thứ hạng chính xác từ tốt nhất đến kém nhất, do đó ranh giới giữa những người tham gia đủ tiêu chuẩn và không đủ tiêu chuẩn trở nên liền kề nhau trong mảng. 
4. Xác định điểm ở chỉ số`k - 1`(lập chỉ mục dựa trên 0). Đây là điểm thấp nhất trong nhóm đủ điều kiện nếu chúng tôi đứng đầu`k`. 
5. Xác định điểm ở chỉ số`k`. Đây là số điểm cao nhất trong nhóm không đủ tiêu chuẩn. 
6. So sánh hai giá trị này. Nếu chúng bằng nhau thì ranh giới nằm trong một nhóm hòa và chúng ta không thể tách những người tham gia một cách rõ ràng. Nếu không, sự phân chia là hợp lệ. 
7. Đầu ra`"EMPATE"`khi ranh giới không rõ ràng và`"BIEN"`khi nó sạch sẽ. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, tất cả các giá trị bằng nhau sẽ tạo thành các khối liền kề nhau. Bất kỳ lựa chọn hợp lệ nào ở trên cùng`k`các phần tử phải lấy tiền tố của mảng được sắp xếp này trừ khi ranh giới nằm bên trong một trong các khối này. Nếu`k`-th và`(k+1)`-các phần tử thứ bằng nhau, phần cắt tiền tố sẽ chia tách một khối có điểm số giống hệt nhau, nghĩa là tồn tại nhiều lựa chọn hợp lệ cho người ở trong hoặc ngoài đầu`k`. Nếu chúng khác nhau thì mọi phần tử trong tiền tố sẽ thống trị hoàn toàn mọi phần tử bên ngoài, do đó phân vùng được xác định duy nhất chỉ bằng điểm số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    
    arr.sort(reverse=True)
    
    if arr[k - 1] == arr[k]:
        print("EMPATE")
    else:
        print("BIEN")
```Giải pháp đọc từng trường hợp kiểm thử, sắp xếp điểm theo thứ tự giảm dần và sau đó thực hiện một so sánh duy nhất tại ranh giới giới hạn. Chi tiết triển khai quan trọng đang sử dụng`k - 1`Và`k`do lập chỉ mục dựa trên 0. Đây là nguồn lỗi phổ biến nhất trong vấn đề này vì việc trộn các vị trí dựa trên 1 và 0 sẽ làm dịch chuyển ranh giới không chính xác. 

Sắp xếp theo thứ tự ngược lại đảm bảo điểm cao nhất nằm ở phía trước, xếp đầu`k`tiếp giáp. Không cần cấu trúc dữ liệu bổ sung vì chỉ có hai vị trí quan trọng sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 5, k = 2, scores = [10, 10, 10, 5, 1]`| Bước | Điểm được sắp xếp | mảng[k-1] | mảng[k] | Quyết định | 
| --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [10, 10, 10, 5, 1] ​​| 10 | 10 | NHẬN XÉT | 

Điều này cho thấy một sự hòa ở ranh giới. Yếu tố thứ hai và thứ ba giống hệt nhau nên không có cách duy nhất nào để chọn chính xác hai người tham gia từ nhóm đứng đầu. 

### Ví dụ 2 

đầu vào:`n = 5, k = 3, scores = [8, 13, 10, 14, 1]`| Bước | Điểm được sắp xếp | mảng[k-1] | mảng[k] | Quyết định | 
| --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [14, 13, 10, 8, 1] | 10 | 8 | BIÊN | 

Ở đây ranh giới nằm giữa 10 và 8, khác biệt. Top 3 được xác định duy nhất chỉ bằng điểm số. 

Những dấu vết này xác nhận rằng chỉ có sự bằng nhau của cặp biên là quan trọng và thứ tự bên trong bên trong các khối có giá trị bằng nhau không ảnh hưởng đến quyết định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n log n) | Mỗi trường hợp thử nghiệm sắp xếp`n`điểm | 
| Không gian | O(n) | Lưu trữ danh sách điểm cho mỗi trường hợp kiểm tra | 

Các ràng buộc cho phép tối đa 10.000 phần tử cho mỗi trường hợp thử nghiệm, do đó việc sắp xếp đủ hiệu quả ngay cả trong trường hợp xấu nhất. Thuật toán thực hiện một lượng nhỏ công việc không đổi sau khi sắp xếp, giữ thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        arr.sort(reverse=True)
        out.append("EMPATE" if arr[k-1] == arr[k] else "BIEN")
    return "\n".join(out) + "\n"

# provided sample
assert run("""5
2 1
1 1
10 5
5 8 0 9 2 0 3 0 1 1
5 1
8 13 10 14 1
3 1
0 1 1
10 8
5 7 4 2 3 2 2 7 7 3
""") == """EMPATE
BIEN
BIEN
EMPATE
EMPATE
"""

# all equal values
assert run("""1
5 3
7 7 7 7 7
""") == "EMPATE\n"

# clear separation
assert run("""1
6 3
9 8 7 6 5 4
""") == "BIEN\n"

# boundary tie only
assert run("""1
4 2
10 10 5 1
""") == "EMPATE\n"

# minimum n
assert run("""1
2 1
1 0
""") == "BIEN\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | NHẬN XÉT | nhóm liên kết đầy đủ khiến cho việc cắt giảm trở nên mơ hồ | 
| giảm nghiêm ngặt | BIÊN | ranh giới rõ ràng sau khi sắp xếp | 
| cà vạt ranh giới | NHẬN XÉT | giá trị bằng nhau trên điểm cắt | 
| kích thước tối thiểu | BIÊN | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Khi tất cả các điểm đều giống nhau, việc sắp xếp sẽ tạo ra một mảng thống nhất. Đối với bất kỳ`k`, cặp`arr[k-1]`Và`arr[k]`bằng nhau, do đó thuật toán đưa ra chính xác`"EMPATE"`. Điều này phản ánh rằng không có phân vùng dựa trên điểm số nào có thể tách rời người tham gia. 

Khi việc cắt xảy ra bên trong một nhóm liên kết, chẳng hạn như`[10, 10, 10, 5]`với`k = 2`, sắp xếp sản lượng`[10, 10, 10, 5]`và sự so sánh là giữa`10`Và`10`, sản xuất`"EMPATE"`. Điều này phù hợp với thực tế là bất kỳ sự lựa chọn nào của hai trong số ba điểm cao nhất đều không thể phân biệt được chỉ dựa vào điểm số. 

Khi có sự sụt giảm nghiêm trọng ở ranh giới, chẳng hạn như`[9, 8, 7, 6]`với`k = 2`, sự so sánh là giữa`8`Và`7`, sản xuất`"BIEN"`. Tiền tố kích thước`k`được xác định duy nhất vì mỗi điểm được chọn hoàn toàn lớn hơn mọi điểm bị loại trừ.
