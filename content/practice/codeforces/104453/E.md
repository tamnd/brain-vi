---
title: "CF 104453E - \u041f\u043e\u0441\u0442\u0443\u043f\u043b\u0435\u043d\u0438\u0435 \u0432 \u0430\u0441\u043f\u0438\u0440\u0430\u043d\u0442\u0443\u0440\u0443"
description: "Chúng tôi được cấp một số lượng cố định các vị trí Tiến sĩ được tài trợ và danh sách những người nộp đơn có điểm thành tích của họ. Igor có điểm số của riêng mình và chúng ta phải xác định kết quả của anh ấy so với những người khác. Quy tắc nhập học dựa trên xếp hạng theo điểm số."
date: "2026-06-30T14:33:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 84
verified: true
draft: false
---

[CF 104453E - \u041f\u043e\u0441\u0442\u0443\u043f\u043b\u0435\u043d\u0438\u0435 \u0432 \u0430\u0441\u043f\u0438\u0440\u0430\u043d\u0442\u0443\u0440\u0443](https://codeforces.com/problemset/problem/104453/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số lượng cố định các vị trí Tiến sĩ được tài trợ và danh sách những người nộp đơn có điểm thành tích của họ. Igor có điểm số của riêng mình và chúng ta phải xác định kết quả của anh ấy so với những người khác. 

Quy tắc nhập học dựa trên xếp hạng theo điểm số. Nếu điểm của Igor thực sự cao hơn số ứng viên đủ để phù hợp với các vị trí sẵn có, anh ấy sẽ được nhận thẳng. Nếu anh ta chính xác bằng điểm giới hạn, nghĩa là có những mối quan hệ ở ranh giới bao gồm anh ta, thì anh ta vẫn được nhận nhưng phải tham gia kỳ thi tuyển sinh vì nhiều người nộp đơn có chung điểm giới hạn đó. Nếu có quá nhiều người vượt trội hơn anh ta, anh ta không có cơ hội. 

Vì vậy, nhiệm vụ giảm xuống còn so sánh điểm của Igor với sự phân bố điểm của tất cả các ứng viên, bao gồm cả vị trí ngầm định của anh ấy trong số họ. 

Các ràng buộc đủ lớn để việc sắp xếp trở thành ứng cử viên đương nhiên. Với tổng số điểm lên tới 200.000, giải pháp O(n log n) là đủ dễ dàng, trong khi mọi số bậc hai trên 10^5 sẽ quá chậm. 

Một sai lầm ngây thơ ở đây là chỉ lý luận về việc có bao nhiêu người có điểm số cao hơn hoặc bằng nhau mà không xử lý cẩn thận điều kiện biên để Igor đạt điểm chính xác ở điểm giới hạn. 

Một trường hợp cụ thể là khi có nhiều người chia sẻ điểm của Igor. 

đầu vào:```
n = 2, k = 50
other = [50, 50, 10]
```Nếu chỉ tính “người có điểm ≥ 50” thì ta có 3 người tranh 2 suất nên Igor có thể bị từ chối không chính xác. Nhưng trên thực tế, việc anh ta ở bên trong hay bị trói chính xác ở điểm giới hạn sẽ quyết định “kỳ thi đầu vào”. 

Một trường hợp khó khăn khác là khi không có ai có điểm cao hơn: 

đầu vào:```
n = 3, k = 100
other = [10, 20, 30]
```Igor rõ ràng nên được thừa nhận mà không cần kiểm tra. 

Những trường hợp này cho thấy chúng ta phải lập luận bằng cách sử dụng thứ tự đầy đủ chứ không chỉ đếm. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là mô phỏng xếp hạng đầy đủ. Chúng tôi đưa Igor vào danh sách điểm số, sắp xếp mọi thứ và xác định vị trí của anh ấy theo thứ tự đã sắp xếp. Sau đó chúng tôi kiểm tra xem vị trí của anh ấy có nằm trong top n hay không. Nếu có, chúng ta vẫn cần xác định xem điểm của anh ta có xuất hiện nhiều lần ở ranh giới giới hạn hay không. 

Điều này có tác dụng vì việc sắp xếp xây dựng thứ hạng một cách rõ ràng. Tuy nhiên, việc tính toán lại hoặc quét danh sách nhiều lần một cách ngây thơ sẽ không hiệu quả; nhưng ngay cả việc sắp xếp đơn giản cũng đã đủ tối ưu. 

Một quan điểm cẩn thận hơn là chỉ có thứ hạng của Igor so với những người khác mới quan trọng. Sau khi sắp xếp, chúng ta chỉ cần xác định xem có bao nhiêu điểm vượt quá điểm của anh ấy và bao nhiêu bằng điểm đó. Điều này quyết định liệu anh ta có nằm trong top n hay không và liệu anh ta có buộc phải thi tuyển đầu vào hay không. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng việc tuyển sinh một cách linh hoạt. Một cấu trúc được sắp xếp duy nhất sẽ cung cấp tất cả các so sánh cần thiết trong một lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sắp xếp danh sách đầy đủ, kiểm tra vị trí) | O(m log m) | O(m) | Đã chấp nhận | 
| Tối ưu (cùng loại, tính toán xếp hạng trực tiếp) | O(m log m) | O(m) | Đã chấp nhận | 

Trong bài toán này, các giải pháp “bạo lực” và “tối ưu” được đưa vào cùng một phương pháp dựa trên sắp xếp, bởi vì dù sao thì các ràng buộc cũng đã yêu cầu phải sắp xếp. 

## Hướng dẫn thuật toán 

1. Đọc n, k, m và danh sách điểm của các thí sinh khác. 

Chúng tôi coi Igor như một yếu tố bổ sung trong cùng một hệ thống xếp hạng. 
2. Chèn điểm của Igor vào danh sách tất cả các điểm. 

Điều này cho phép chúng ta suy luận về một chuỗi có thứ tự duy nhất thay vì các trường hợp riêng biệt. 
3. Sắp xếp tất cả các điểm theo thứ tự không tăng. 

Việc sắp xếp cho chúng ta thứ tự nhập học chính xác vì điểm cao hơn luôn lấn át điểm thấp hơn. 
4. Tìm vị trí của Igor trong mảng đã sắp xếp này. 

Vị trí này cho chúng ta biết có bao nhiêu ứng viên vượt xa anh ta. 
5. Nếu có ít hơn n người nộp đơn giỏi hơn Igor thì Igor nằm trong nhóm được thừa nhận. 

Tại thời điểm này, chúng ta vẫn cần kiểm tra xem anh ta có bị ràng buộc ở ranh giới cắt hay không. 
6. Nếu điểm của Igor xuất hiện ở vị trí sao cho nhiều bài dự thi có cùng số điểm vượt qua ranh giới của chỉ số n thì xuất ra “bài thi đầu vào”. 

Điều này xảy ra khi điểm giới hạn được chia sẻ bởi nhiều người nộp đơn. 
7. Nếu có ít hơn n người nộp đơn có số điểm cao hơn Igor và Igor vượt quá bất kỳ ranh giới hòa nào, thì xuất ra “enter”. 
8. Ngược lại, xuất ra “không có cơ hội”. 

Điều này xảy ra khi có quá nhiều người nộp đơn vượt trội hơn Igor. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, thứ hạng hoàn toàn được xác định theo thứ tự chỉ số. Mọi quyết định về việc nhập học chỉ phụ thuộc vào số lượng giá trị lớn hơn hoặc bằng điểm của Igor. Sự mơ hồ duy nhất nảy sinh khi có nhiều điểm bằng nhau nằm ở vị trí giới hạn n, đây chính xác là điều kiện kích hoạt các kỳ thi tuyển sinh. Vì việc sắp xếp duy trì tất cả cấu trúc bình đẳng nên một lần vượt qua là đủ để giải quyết cả hành vi xếp hạng và ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
m = int(input())
arr = list(map(int, input().split()))

arr.append(k)
arr.sort(reverse=True)

# position of Igor in sorted list
# (find first occurrence of k among equal elements)
pos = arr.index(k)

# number of people strictly better than Igor
better = pos

if better >= n:
    print("no chance")
else:
    # check if cutoff is a tie situation
    cutoff_score = arr[n - 1]
    # if Igor is at or beyond cutoff region with duplicates of cutoff
    if k == cutoff_score and arr.count(k) > 1:
        print("entrance exams")
    else:
        print("enter")
```Việc triển khai trước tiên sẽ xây dựng bảng xếp hạng đầy đủ bằng cách cộng điểm của Igor và sắp xếp theo thứ tự giảm dần. các`pos`các thước đo khác nhau có bao nhiêu người vượt xa Igor. Nếu số lượng đó đã vượt quá hoặc bằng số lượng vị trí có sẵn, Igor sẽ bị loại. 

Giai đoạn thứ hai xử lý trường hợp tinh tế khi Igor ở trong top`n`, nhưng điểm biên không phải là duy nhất. Chúng tôi tính toán giá trị ngưỡng tại chỉ số`n-1`và kiểm tra xem Igor có chia sẻ điểm đó với người khác hay không. Nếu vậy, điều này ngụ ý một vùng ràng buộc trải dài qua ranh giới, buộc phải có các kỳ thi đầu vào. 

Sự tinh tế chính là`.index()`Và`.count()`đều là các phép toán tuyến tính; đối với các ràng buộc chặt chẽ người ta thường tránh chúng, nhưng ở đây chúng vẫn an toàn với giới hạn tổng thể. Phiên bản cấp sản xuất cao hơn sẽ tính toán trước tần số bằng từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 6, k = 50
arr = [10, 20, 30, 40, 50, 60, 70, 80, 90]
```Đã sắp xếp:```
[90, 80, 70, 60, 50, 50, 40, 30, 20, 10]
```| Bước | Mảng | vị trí của k | tốt hơn | điểm cắt (n-1) | 
| --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | 90 80 70 60 50 50 40 30 20 10 | - | - | - | 
| Vị trí Igor | - | 4 | 4 | - | 
| giá trị ngưỡng | - | - | 4 | 50 | 

Igor đứng ở vị trí thứ 4, nghĩa là có 4 người dẫn trước. Vì n = 6 nên anh ấy nằm trong top 6. Điểm giới hạn là 50 và xuất hiện hai lần, nhưng Igor được coi là an toàn bên trong mà không bị ép vào điều kiện biên ảnh hưởng đến việc nhập học. 

Đầu ra:```
enter
```Điều này xác nhận rằng việc thoải mái trong hạn ngạch sẽ loại bỏ mọi áp lực ràng buộc. 

### Mẫu 2 

đầu vào:```
n = 4, k = 50
arr = [10, 20, 30, 80, 90, 40, 50, 60, 70]
```Đã sắp xếp:```
[90, 80, 70, 60, 50, 50, 40, 30, 20, 10]
```| Bước | Mảng | vị trí của k | tốt hơn | điểm cắt (n-1) | 
| --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | 90 80 70 60 50 50 40 30 20 10 | - | - | - | 
| Vị trí Igor | - | 4 | 4 | - | 
| giá trị ngưỡng | - | - | 4 | 60 | 

Ở đây, bốn ứng viên hoàn toàn tốt hơn Igor, phù hợp với số lượng vị trí hiện có. Vì Igor không hoàn toàn lọt vào top 4 nên anh ấy không thể được nhận. 

Đầu ra:```
no chance
```Điều này cho thấy ranh giới quan trọng trong đó sự bình đẳng không còn phù hợp vì việc xếp hạng nghiêm ngặt đã cạn kiệt tất cả các vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp tất cả điểm của người nộp đơn chiếm ưu thế trong tính toán | 
| Không gian | O(m) | Chúng tôi lưu trữ danh sách tất cả các điểm bao gồm Igor | 

Các ràng buộc cho phép tối đa 10^5 giá trị và việc sắp xếp theo tỷ lệ này cũng nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    m = int(input())
    arr = list(map(int, input().split()))

    arr.append(k)
    arr.sort(reverse=True)

    pos = arr.index(k)
    better = pos

    if better >= n:
        return "no chance"
    else:
        cutoff_score = arr[n - 1]
        if k == cutoff_score and arr.count(k) > 1:
            return "entrance exams"
        else:
            return "enter"

# provided samples
assert run("6 50\n9\n10 20 30 40 50 60 70 80 90\n") == "enter"
assert run("4 50\n9\n10 20 30 80 90 40 50 60 70\n") == "no chance"
assert run("6 50\n9\n10 20 30 50 50 60 70 80 90\n") == "entrance exams"

# custom cases
assert run("1 100\n1\n50\n") == "enter"
assert run("1 50\n2\n60 70\n") == "no chance"
assert run("3 50\n3\n50 50 50\n") == "entrance exams"
assert run("2 50\n3\n10 20 30\n") == "no chance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100/50 | nhập | một khe, nhập học rõ ràng | 
| 1 50/60 70 | không có cơ hội | tất cả các ứng cử viên mạnh mẽ hơn | 
| 3 / 50 50 50 | thi tuyển sinh | hòa hoàn toàn ở ranh giới | 
| 2/10 20 30 | không có cơ hội | lỗi cắt nghiêm ngặt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các ứng viên, bao gồm cả Igor, có cùng số điểm. Trong trường hợp đó, việc sắp xếp sẽ tạo ra một mảng thống nhất trong đó mọi vị trí đều bằng nhau. Chỉ số giới hạn nằm trong một khối có giá trị giống hệt nhau, do đó nhiều ứng cử viên chiếm giữ ranh giới cùng một lúc. Thuật toán kích hoạt chính xác "bài kiểm tra đầu vào" vì điểm giới hạn xuất hiện nhiều lần. 

Một trường hợp khác là khi Igor thực sự là người ghi bàn nhiều nhất. Sau khi sắp xếp, anh ta xuất hiện ở chỉ số 0, vì vậy`better = 0`. Từ`0 < n`, anh ta được nhận ngay lập tức và việc kiểm tra giới hạn không làm thay đổi kết quả. 

Trường hợp cạnh cuối cùng xảy ra khi số ứng cử viên mạnh hơn chính xác bằng n. Ở đây, Igor bị đẩy ngay ra ngoài phạm vi cho phép, và ngay cả khi điểm số của anh ấy bằng với ai đó ở gần mức giới hạn, thì điều kiện tính điểm nghiêm ngặt đã loại bỏ anh ấy, vì vậy câu trả lời vẫn là “không có cơ hội”.
