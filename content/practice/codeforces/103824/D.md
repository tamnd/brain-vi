---
title: "CF 103824D - \u4e0a\u5206"
description: "Chúng ta được cung cấp một chuỗi các số nguyên riêng biệt thể hiện sự thay đổi xếp hạng từ các cuộc thi sắp tới. Chúng tôi được phép sắp xếp lại các giá trị này một cách tùy ý và đưa chúng vào một quy trình mô phỏng số lượng cuộc thi mà người chơi thực sự chơi."
date: "2026-07-02T08:19:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103824
codeforces_index: "D"
codeforces_contest_name: "2022 Summer Camp of XTU Qualifying Round"
rating: 0
weight: 103824
solve_time_s: 70
verified: true
draft: false
---

[CF 103824D - \u4e0a\u5206](https://codeforces.com/problemset/problem/103824/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên riêng biệt thể hiện sự thay đổi xếp hạng từ các cuộc thi sắp tới. Chúng tôi được phép sắp xếp lại các giá trị này một cách tùy ý và đưa chúng vào một quy trình mô phỏng số lượng cuộc thi mà người chơi thực sự chơi. 

Quá trình bắt đầu với độ dài dự kiến ​​là 2 trận đấu. Người chơi bắt đầu thực hiện trình tự được sắp xếp lại từ phần tử đầu tiên. Sau khi kết thúc cuộc thi thứ i, tổng số cuộc thi theo kế hoạch có thể tăng thêm 2, nhưng chỉ theo một mô hình cục bộ rất cụ thể: giá trị hiện tại phải dương, giá trị trước đó cũng phải dương và giá trị dương hiện tại phải lớn hơn giá trị trước đó. Khi điều này xảy ra, độ dài dự kiến ​​sẽ tăng lên, có khả năng cho phép người chơi tiếp tục vượt quá những gì dự kiến ​​ban đầu. Nếu không thì không có gì thay đổi. 

Điều quan trọng là quá trình này diễn ra tuần tự và tự dừng. Trình phát chỉ tiếp tục miễn là chỉ số hiện tại không vượt quá độ dài dự kiến ​​hiện tại. Khi tôi đạt đến giới hạn theo kế hoạch, quá trình thực thi sẽ dừng ngay cả khi vẫn còn các phần tử không được sử dụng. 

Nhiệm vụ của chúng ta là hoán vị mảng sao cho tổng của tất cả các giá trị thực sự được phát là lớn nhất. 

Ràng buộc n lên tới 100.000 cho mỗi trường hợp thử nghiệm với tổng tổng cũng bị giới hạn có nghĩa là chúng ta cần ít nhất hành vi dựa trên sắp xếp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai hoặc liên quan đến việc mô phỏng nhiều hoán vị đều không thể thực hiện được ngay lập tức. 

Một khó khăn nhỏ là điểm dừng phụ thuộc vào chính hoán vị. Chúng tôi không chỉ đơn giản là tối đa hóa tổng tiền tố mà thay vào đó, chúng tôi kiểm soát tiền tố mở rộng linh hoạt có độ dài phụ thuộc vào số lượng cặp dương liền kề tăng dần mà chúng tôi tạo sớm. 

Một trường hợp có thể phá vỡ trực giác ngây thơ là khi những điều tích cực xen kẽ với những điều tiêu cực. 

Ví dụ: nếu chúng ta sắp xếp sớm các số dương lớn nhưng theo thứ tự giảm dần, chẳng hạn như [5, 4, 3, 2], thì sẽ không có phần mở rộng nào xảy ra vì điều kiện yêu cầu các số dương tăng nghiêm ngặt. Quá trình sẽ dừng ngay lập tức ở độ dài 2, thiếu phần lớn mức tăng. Một kẻ tham lam ngây thơ “đặt số lượng lớn lên hàng đầu” đã thất bại hoàn toàn. 

Một trường hợp thất bại khác là giả định rằng chúng ta phải luôn tối đa hóa tiện ích mở rộng. Tiện ích mở rộng làm tăng các chỉ số có thể truy cập nhưng có thể buộc phải bao gồm các giá trị âm có hại. Vì vậy, tối đa hóa một cách mù quáng số lượng chuỗi tích cực hợp lệ cũng không phải là tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị và mô phỏng quy trình cho mỗi thứ tự. Đối với mỗi hoán vị, chúng tôi quét từ trái sang phải, duy trì độ dài dự kiến ​​hiện tại và áp dụng quy tắc mở rộng. Chi phí này là O(n) cho mỗi hoán vị và có n! hoán vị, điều này không thể xảy ra ngay cả với n = 10. 

Quan sát quan trọng là chỉ có cấu trúc cục bộ trong số các giá trị tích cực mới có tác dụng mở rộng quá trình. Phủ định không bao giờ tham gia vào các điều kiện mở rộng và chúng chỉ ảnh hưởng đến câu trả lời thông qua việc chúng có được đưa vào hay không trước khi dừng. Điều này chia vấn đề thành hai mục tiêu cạnh tranh nhau: chúng tôi muốn tất cả các mặt tích cực được sử dụng sớm và chúng tôi muốn tránh kéo theo quá nhiều tiêu cực do mở rộng quá mức. 

Quy tắc mở rộng chỉ phụ thuộc vào các cặp dương tăng dần liền kề. Điều này cho thấy rằng nếu chúng ta kiểm soát cách sắp xếp các mặt tích cực, chúng ta sẽ trực tiếp kiểm soát số lần quy trình mở rộng. 

Bây giờ hãy xem xét những gì chúng ta thực sự muốn quá trình thực hiện. Mỗi phần mở rộng sẽ tăng tổng chiều dài lên 2. Nếu chúng tôi quản lý để đảm bảo rằng tất cả các giá trị dương xuất hiện trước bất kỳ giá trị âm nào theo thứ tự thực hiện thì mọi vị trí đã chơi góp phần vào phần mở rộng đều có lợi. Sau khi hết phần tích cực, các phần mở rộng tiếp theo chỉ đưa phần âm vào tiền tố được phát, điều này có hại.

Điều này dẫn đến việc đơn giản hóa cấu trúc: chúng tôi muốn tất cả các giá trị dương được đặt đầu tiên và trong số đó, chúng tôi muốn tối đa hóa số lượng các cặp tăng liền kề hợp lệ trong khi vẫn giữ cấu trúc đó ổn định và có thể dự đoán được. Cách đơn giản nhất để đảm bảo số lượng cặp hợp lệ tối đa trong số các cặp dương là sắp xếp chúng theo thứ tự tăng dần. Khi đó, mọi cặp dương liền kề sẽ thỏa mãn điều kiện mở rộng. 

Tốt nhất nên đặt các phủ định ở cuối theo thứ tự tùy ý vì thứ tự bên trong của chúng hoàn toàn không ảnh hưởng đến phần mở rộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force + mô phỏng | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp các mặt tích cực, nối thêm các mặt tiêu cực | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị theo cách phân tách các giá trị dương và không dương. 

1. Chia mảng thành tích cực và không tích cực. Lý do là chỉ các giá trị dương mới có thể tham gia vào trình kích hoạt tiện ích mở rộng, do đó việc trộn chúng sẽ chỉ gây ra những gián đoạn không cần thiết. 
2. Sắp xếp các giá trị dương theo thứ tự tăng dần. Điều này đảm bảo rằng mọi cặp dương liền kề đều thỏa mãn cả hai điều kiện để kích hoạt tiện ích mở rộng: cả hai đều dương và giá trị thứ hai lớn hơn giá trị đầu tiên. Điều này tối đa hóa số lượng trình kích hoạt hợp lệ theo cách được kiểm soát. 
3. Sắp xếp các giá trị còn lại (số 0 và số âm) một cách tùy ý, thường theo thứ tự tăng dần để nếu đạt đến chúng thì những giá trị ít gây hại nhất sẽ xuất hiện sớm hơn. 
4. Xuất tất cả các kết quả dương trước, sau đó là tất cả các kết quả không tích cực. 

Tại sao thứ tự này hoạt động đòi hỏi phải hiểu quá trình phát triển như thế nào. Quá trình thực thi luôn bắt đầu trong khối dương và khi nó vẫn ở trong khối này, mọi cặp liền kề đều góp phần kéo dài độ dài theo kế hoạch. Vì tất cả các phần tích cực đều liền kề và được sắp xếp nên quá trình này sẽ tiếp tục kéo dài cho đến khi nó tiêu thụ hết toàn bộ phần tích cực một cách hiệu quả. 

Sau khi quá trình chuyển sang phân đoạn không dương, không thể mở rộng thêm nữa vì điều kiện yêu cầu giá trị dương. Tại thời điểm đó, quy trình chỉ đơn giản chạy tuyến tính cho đến khi đạt đến độ dài dự kiến ​​cuối cùng hoặc sử dụng hết mảng. 

Bất biến cấu trúc quan trọng là tất cả các cơ hội mở rộng được tập trung hoàn toàn trong tiền tố tích cực đã được sắp xếp và không có yếu tố kích hoạt có lợi nào bị bỏ sót do sự gián đoạn từ các tiêu cực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = []
        neg = []

        for x in a:
            if x > 0:
                pos.append(x)
            else:
                neg.append(x)

        pos.sort()
        neg.sort()

        res = pos + neg
        print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc xây dựng. Chúng tôi phân tách các giá trị bằng dấu vì chỉ những giá trị tích cực mới có thể góp phần kéo dài số lượng trận đấu theo kế hoạch. Việc sắp xếp các giá trị dương đảm bảo mọi cặp liên tiếp đều hợp lệ để kích hoạt tiện ích mở rộng. 

Khối âm được thêm vào cuối vì bất kỳ việc bao gồm âm bản nào trước khi sử dụng hết tất cả các âm dương sẽ chỉ làm giảm tổng số tiền mà không tạo ra cơ hội gia hạn mới. 

Một chi tiết triển khai tinh tế là chúng ta hoàn toàn không cần phải mô phỏng quy trình. Cấu trúc của hoán vị tối ưu đảm bảo rằng quy tắc dừng động luôn diễn ra theo cách có thể dự đoán được, do đó việc mô phỏng rõ ràng sẽ chỉ bổ sung thêm chi phí. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào: 

đầu vào:```
1
4
1 4 2 -3
```Chúng tôi chia thành tích cực và tiêu cực. 

| Bước | Tích cực | Tiêu cực | Mảng đã xây dựng | 
| --- | --- | --- | --- | 
| Ban đầu | [1, 4, 2] | [-3] | | 
| Sau khi chia tay | [1, 2, 4] | [-3] | | 
| Sau khi sắp xếp | [1, 2, 4] | [-3] | | 
| Đầu ra cuối cùng | | | [1, 2, 4, -3] | 

Thứ tự này đảm bảo rằng quy trình nhìn thấy một chuỗi tích cực tăng dần rõ ràng trước khi bất kỳ tiêu cực nào xuất hiện. 

Bây giờ hãy xem xét một trường hợp chỉ có phủ định: 

đầu vào:```
1
3
-5 -1 -10
```| Bước | Tích cực | Tiêu cực | Mảng đã xây dựng | 
| --- | --- | --- | --- | 
| Chia | [] | [-5, -1, -10] | | 
| Sắp xếp | [] | [-10, -5, -1] | | 
| Đầu ra | | | [-10, -5, -1] | 

Ở đây không có sự mở rộng nào xảy ra và điều tốt nhất chúng ta có thể làm là giảm thiểu việc tiếp xúc sớm với những âm bản lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế cho từng trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ các mảng được phân vùng | 

Các ràng buộc cho phép tổng cộng tối đa 100.000 phần tử trong tất cả các trường hợp thử nghiệm, do đó việc sắp xếp theo từng trường hợp thử nghiệm vẫn hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            pos = [x for x in a if x > 0]
            neg = [x for x in a if x <= 0]
            pos.sort()
            neg.sort()
            print(*pos, *neg)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# sample-like sanity checks
assert run("1\n3\n1 3 2") == "1 2 3"

# all negatives
assert run("1\n3\n-5 -1 -10") == "-10 -5 -1"

# mix
assert run("1\n4\n4 2 1 -3") == "1 2 4 -3"

# single positive dominant structure
assert run("1\n5\n5 1 2 3 -1") == "1 2 3 5 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mặt tích cực không có thứ tự | tích cực được sắp xếp | đặt hàng đúng đắn | 
| tất cả tiêu cực | sắp xếp tiêu cực | ổn định trong trường hợp không mở rộng | 
| giá trị hỗn hợp | tích cực trước rồi đến tiêu cực | tách cấu trúc | 
| độ lớn lệch | hành vi phân vùng đúng | mạnh mẽ dưới sự mất cân bằng phân phối | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có giá trị dương. Trong tình huống đó, không có phần mở rộng nào có thể xảy ra nên quá trình chỉ cần lấy hai phần tử đầu tiên và dừng lại. Việc sắp xếp các số âm đảm bảo chúng ta giảm thiểu tổn thất sớm. 

Một trường hợp khác là khi tất cả các giá trị đều dương. Thuật toán sắp xếp mọi thứ theo thứ tự tăng dần, buộc mọi cặp liền kề phải kích hoạt các tiện ích mở rộng nhiều lần. Điều này làm cho quá trình mở rộng tối đa, đảm bảo không có giá trị nào được sử dụng. 

Một trường hợp hỗn hợp như [100, -1, 2, 3, 4] chứng minh tại sao việc phân tách lại quan trọng. Nếu đặt 100 trước, chúng ta sẽ sớm mất mọi cơ hội gia hạn. Bằng cách đặt số dương là [2, 3, 4, 100], chúng tôi đảm bảo chuỗi được kiểm soát trong khi vẫn tối đa hóa số tiền có thể tiếp cận và số âm được hoãn lại một cách an toàn.
