---
title: "CF 105228D - Frogo"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có $n$ bước, mỗi bước có giá trị chiều cao và một con ếch di chuyển từ bước ngoài cùng bên trái sang bước ngoài cùng bên phải. Con ếch có thể sắp xếp lại các bước theo thứ tự bất kỳ trước khi bắt đầu hành trình."
date: "2026-06-24T16:18:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105228
codeforces_index: "D"
codeforces_contest_name: "SanSi Cup 2023"
rating: 0
weight: 105228
solve_time_s: 88
verified: false
draft: false
---

[CF 105228D - Frogo](https://codeforces.com/problemset/problem/105228/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$các bước, mỗi bước có một giá trị chiều cao và một con ếch di chuyển từ bước ngoài cùng bên trái đến bước ngoài cùng bên phải. Con ếch có thể sắp xếp lại các bước theo thứ tự bất kỳ trước khi bắt đầu hành trình. 

Quy tắc di chuyển dựa trên sự chênh lệch độ cao giữa các bước liên tiếp. Nếu ếch nhảy lên trên thì độ cao tăng lên không được vượt quá$k$. Nếu giảm xuống thì mức giảm không được vượt quá$k$cũng vậy. Bất kỳ bước nhảy nào có chênh lệch độ cao tuyệt đối vượt quá$k$gây ra thất bại. Câu hỏi đặt ra là liệu chúng ta có thể hoán vị nhiều tập hợp độ cao đã cho để tồn tại thứ tự của tất cả các bước bắt đầu từ bước đầu tiên đến bước cuối cùng trong đó mọi hiệu số liền kề nhiều nhất không?$k$. 

Đầu ra của mỗi trường hợp thử nghiệm là một ký tự đơn cho biết liệu thứ tự đó có tồn tại hay không. 

Các ràng buộc đủ nhỏ để bất kỳ giải pháp nào có thể đạt tới mức gần đúng$O(n \log n)$mỗi trường hợp thử nghiệm là tốt. Vì tổng cộng$n$trên các trường hợp thử nghiệm nhiều nhất là 2000, thậm chí việc sắp xếp từng thử nghiệm một cách độc lập cũng đủ nhanh. Bất cứ điều gì bậc hai cho mỗi bài kiểm tra vẫn được chấp nhận trong thực tế, nhưng chúng ta nên tránh thử tất cả các hoán vị hoặc quay lui vì điều đó sẽ tăng lên giai thừa. 

Một vấn đề tế nhị là việc sắp xếp lại sẽ loại bỏ hoàn toàn cấu trúc vị trí. Đây là sự thay đổi quan trọng: chúng tôi không giải quyết vấn đề truyền tải mảng mà đang xây dựng một hoán vị với các hiệu liền kề bị chặn. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là cho rằng chúng ta chỉ cần kiểm tra xem khoảng cách tối đa giữa các phần tử được sắp xếp liên tiếp có nhiều nhất không$k$. Điều đó thực sự đúng ở đây, nhưng nhiều người suy nghĩ quá nhiều và thử các mô phỏng hoặc kết nối đồ thị tham lam. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử tất cả các hoán vị của độ cao và kiểm tra xem mỗi hoán vị có thỏa mãn ràng buộc mà mọi sai phân liền kề nhiều nhất hay không$k$. Điều này đúng nhưng ngay lập tức không khả thi vì có$n!$hoán vị, vượt xa giới hạn ngay cả đối với$n = 10$. 

Chúng ta cần hiểu tính chất nào của một hoán vị làm cho tất cả các sai phân liền kề bị giới hạn bởi$k$. Khi mảng được sắp xếp, bước nhảy lớn nhất có thể giữa các phần tử được chọn liên tiếp sẽ được giảm thiểu. Bất kỳ thứ tự nào khác chỉ có thể làm tăng một số khoảng cách liền kề so với thứ tự đã sắp xếp, bởi vì việc sắp xếp đặt các giá trị gần nhau càng chặt chẽ càng tốt. 

Điều này dẫn đến cái nhìn sâu sắc quan trọng: nếu chúng ta sắp xếp mảng, sự khác biệt gần kề tồi tệ nhất theo thứ tự được sắp xếp đó chính xác là khoảng cách tối đa giữa các phần tử liên tiếp. Nếu khoảng cách tối đa đó là nhiều nhất$k$, chúng ta có thể chỉ cần sử dụng thứ tự được sắp xếp làm công trình hợp lệ. Nếu nó vượt quá$k$, thì không có sự sắp xếp lại nào có thể khắc phục được nó, bởi vì hai giá trị đó phải được phân tách bằng ít nhất một giá trị kề ở đâu đó và bất kỳ giá trị kề nào giữa các giá trị phải phát sinh ít nhất khoảng cách đó tại một số điểm trong bất kỳ thứ tự nào. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra một điều kiện sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Sắp xếp và kiểm tra sự khác biệt liền kề |$O(n \log n)$|$O(1)$thêm (hoặc$O(n)$) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc danh sách độ cao và giới hạn$k$. Cấu trúc của bài toán không phụ thuộc vào thứ tự, vì vậy chúng ta ngay lập tức coi đầu vào là một tập hợp nhiều tập. 
2. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp được sử dụng vì nó giảm thiểu sự khác biệt cục bộ giữa các phần tử liên tiếp, đó chính xác là điều con ếch quan tâm. 
3. Quét qua mảng đã sắp xếp và tính chênh lệch tối đa giữa các phần tử liên tiếp. 
4. Nếu tại bất kỳ thời điểm nào có chênh lệch vượt quá$k$, chúng tôi ngay lập tức kết luận rằng không có sự sắp xếp lại hợp lệ nào tồn tại và lỗi đầu ra. Nếu không, chúng tôi kết luận thành công. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Lý do việc quét này là đủ vì thứ tự được sắp xếp là sự sắp xếp các giá trị được “nén” nhất. Bất kỳ hoán vị nào khác đều đưa ra ít nhất một lân cận không nhỏ hơn một trong các lân cận được sắp xếp theo cách không thể cải thiện khoảng cách tồi tệ nhất. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ hoán vị hợp lệ. Nhìn vào các yếu tố nhỏ nhất và lớn nhất. Tại một số điểm trong hoán vị, các chuyển tiếp phải di chuyển giữa các giá trị trung gian. Sự liên kết chặt chẽ nhất có thể giữa các giá trị đạt được bằng cách sắp xếp chúng theo độ lớn. Nếu ngay cả trong chuỗi tối ưu này, khoảng cách liền kề lớn nhất vượt quá$k$, thì cặp giá trị đó phải liền kề ở một nơi nào đó trong bất kỳ quá trình truyền tải nào hoặc phải được bắc cầu bằng các giá trị trung gian mà các chuyển đổi tích lũy của chúng không thể làm giảm yêu cầu nhảy cục bộ tối đa. Do đó, sự sắp xếp được sắp xếp đóng vai trò như cấu hình minimax cho các hiệu liền kề, vì vậy việc kiểm tra nó là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort()
        
        ok = True
        for i in range(n - 1):
            if a[i + 1] - a[i] > k:
                ok = False
                break
        
        out.append("S" if ok else "F")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc sắp xếp từng trường hợp thử nghiệm một cách độc lập. Thao tác chính là một lần duyệt sau khi sắp xếp, trong đó chúng tôi chỉ so sánh các hàng xóm. Biến quyết định`ok`theo dõi xem có vi phạm nào xảy ra không. Việc ngắt sớm không cần thiết để đảm bảo tính chính xác nhưng làm giảm những so sánh không cần thiết trong các đầu vào trong trường hợp xấu nhất. 

Một sai lầm phổ biến là quên rằng chỉ những khác biệt liền kề mới quan trọng sau khi sắp xếp. Không cần phải xem xét các cặp không liền kề vì bất kỳ đường dẫn hợp lệ nào giữa chúng đều đã bị ràng buộc bởi các bước trung gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 3
a = [1, 10, 4, 7, 2]
```Sau khi sắp xếp:```
[1, 2, 4, 7, 10]
```| tôi | một [tôi] | a[i+1] | khác biệt | trạng thái | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | được | 
| 1 | 2 | 4 | 2 | được | 
| 2 | 4 | 7 | 3 | được | 
| 3 | 7 | 10 | 3 | được | 

Tất cả sự khác biệt là 3, vì vậy kết quả đầu ra là hợp lệ. 

Điều này cho thấy rằng mặc dù mảng ban đầu trông có vẻ hỗn loạn nhưng việc sắp xếp sẽ nén nó thành một chuỗi trong đó mọi bước nhảy đều tôn trọng ràng buộc. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 2
a = [1, 5, 6, 9]
```Sau khi sắp xếp:```
[1, 5, 6, 9]
```| tôi | một [tôi] | a[i+1] | khác biệt | trạng thái | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 4 | thất bại | 
| 1 | 5 | 6 | 1 | - | 
| 2 | 6 | 9 | 3 | - | 

Khoảng cách đầu tiên đã vượt quá$k$, do đó không có sự sắp xếp hợp lệ nào tồn tại. 

Điều này chứng tỏ rằng một khoảng trống lớn trong thứ tự sắp xếp sẽ ngay lập tức chặn mọi giải pháp, bất kể chúng ta cố gắng sắp xếp lại các phần tử như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum n \log n)$| Mỗi trường hợp thử nghiệm được sắp xếp độc lập và tổng số$n$nhỏ | 
| Không gian |$O(1)$phụ trợ | Việc sắp xếp được thực hiện tại chỗ ngoài việc lưu trữ đầu vào | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 2000, do đó, ngay cả việc sắp xếp đầy đủ cũng không đáng kể trong thời gian giới hạn. Quá trình quét tuyến tính bổ sung thêm chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-style cases
assert run("""1
5 3
1 10 4 7 2
""") == "S"

assert run("""1
4 2
1 5 6 9
""") == "F"

# minimum size
assert run("""1
2 10
1 100
""") == "S"

# all equal
assert run("""1
5 0
7 7 7 7 7
""") == "S"

# tight chain
assert run("""1
5 1
1 2 3 4 10
""") == "F"

# already valid chain
assert run("""1
6 2
1 3 5 7 9 11
""") == "S"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố cách xa nhau | S | tính khả thi cơ bản chỉ với một lần nhảy | 
| tất cả đều bình đẳng | S | độ ổn định không sai biệt | 
| ngoại lệ lớn | F | chuỗi ngắt quãng đơn | 
| cách đều nhau | S | xây dựng dây chuyền tối ưu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 2$. Trong trường hợp này, câu trả lời chỉ phụ thuộc vào việc liệu chênh lệch tuyệt đối giữa hai giá trị có lớn nhất hay không.$k$. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp hai phần tử và kiểm tra sự khác biệt của chúng hoàn toàn giống nhau về logic. 

Một trường hợp cạnh khác xảy ra khi tất cả các giá trị giống hệt nhau. Việc sắp xếp không tạo ra sự khác biệt nào ở mọi nơi, do đó quá trình quét diễn ra mà không gây ra bất kỳ lỗi nào, trả về thành công một cách chính xác ngay cả khi$k = 0$. 

Một trường hợp minh họa hơn là khi một giá trị nằm xa các giá trị còn lại, chẳng hạn như`[1, 2, 3, 100]`với nhỏ$k$. Sắp xếp sản lượng`[1, 2, 3, 100]`và khoảng cách giữa 3 và 100 ngay lập tức vi phạm ràng buộc. Điều này phản ánh chính xác rằng không có hoán vị nào có thể tránh việc đặt 100 liền kề với một thứ nào đó trong chuỗi tại một thời điểm nào đó, buộc phải có một bước nhảy quá lớn.
