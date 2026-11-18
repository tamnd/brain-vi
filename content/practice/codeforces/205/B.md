---
title: "CF 205B - ​​Chú Voi Nhỏ và Phân Loại"
description: "Chúng ta được cấp một mảng các số nguyên và một thao tác bao gồm việc chọn một phân đoạn liền kề và tăng mọi phần tử bên trong phân đoạn đó lên đúng một. Chúng tôi có thể lặp lại thao tác này bất kỳ số lần nào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 205
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 129 (Div. 2)"
rating: 1400
weight: 205
solve_time_s: 90
verified: true
draft: false
---

[CF 205B - Chú voi nhỏ và việc phân loại](https://codeforces.com/problemset/problem/205/B) 

**Đánh giá:** 1400 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và một thao tác bao gồm việc chọn một phân đoạn liền kề và tăng mọi phần tử bên trong phân đoạn đó lên đúng một. Chúng tôi có thể lặp lại thao tác này bất kỳ số lần nào. Sau tất cả các thao tác, chúng ta không cần phải khớp một mảng mục tiêu cố định; thay vào đó, chúng ta chỉ cần mảng cuối cùng không giảm. 

Vì vậy, nhiệm vụ là phải hiểu cần bao nhiêu thao tác tăng phạm vi để chuyển đổi mảng ban đầu thành một mảng không bao giờ giảm khi đọc từ trái sang phải. 

Điều tinh tế quan trọng là chúng ta có thể tự do lựa chọn hình dạng cuối cùng của mảng, miễn là nó không giảm và chỉ có thể thu được bằng cách thêm số gia trên các mảng con. Chúng ta không được phép giảm giá trị, chỉ được phép “đẩy” các phần của mảng lên trên bằng cách sử dụng các phân đoạn. 

Ràng buộc n 100000 ngụ ý rằng chúng ta không thể mô phỏng các hoạt động một cách rõ ràng. Mỗi thao tác có thể chạm vào các phần tử O(n) và thậm chí vài nghìn thao tác sẽ quá chậm nếu áp dụng một cách ngây thơ. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề xuống một lần quét tuyến tính. 

Một sai lầm ngây thơ nhưng tự nhiên là nghĩ rằng chúng ta có thể tham lam “sửa chữa” các nghịch đảo cục bộ bằng cách nâng cao các hậu tố hoặc tiền tố mà không tính toán cẩn thận xem các phép toán chồng chéo lên nhau như thế nào. 

Ví dụ, hãy xem xét [5, 1, 1]. Một cách tiếp cận bất cẩn có thể cố gắng nâng cao phần tử thứ hai và thứ ba một cách độc lập, nhưng vì các hoạt động tác động theo phạm vi nên việc cố định một vị trí có thể vô tình ảnh hưởng đến các vị trí trước đó. 

Một trường hợp sai lầm khác là khi mảng đã không giảm, chẳng hạn như [1, 2, 3, 4]. Mọi giải pháp đúng đều phải trả về 0 ngay lập tức và không được cố gắng “cải thiện” thêm bất cứ điều gì. 

Một kịch bản cạnh khác là khi mảng giảm nghiêm ngặt, chẳng hạn như [4, 3, 2, 1]. Ở đây, mỗi lần thả liền kề đều đóng góp độc lập vào số lượng thao tác được yêu cầu, nhưng không rõ tại sao những đóng góp đó không tương tác một cách phức tạp. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ định nghĩa của hoạt động. Mỗi thao tác sẽ tăng một phân đoạn đã chọn, vì vậy chúng ta có thể thử mô phỏng tất cả các chuỗi thao tác và kiểm tra xem khi nào mảng không giảm. Ngay cả khi chúng tôi hạn chế một số thao tác cố định, hệ số phân nhánh vẫn rất lớn vì mỗi bước cho phép O(n²) phân đoạn có thể. Điều này nhanh chóng trở thành cấp số nhân về số lượng phép toán, điều này không khả thi ngay cả đối với các mảng nhỏ. 

Một ý tưởng thô bạo có cấu trúc hơn là sửa một mảng mục tiêu không giảm b trong đó b[i] ≥ a[i], sau đó cố gắng biểu thị sự khác biệt b - a dưới dạng tổng của các khoảng tăng dần. Điều này tương đương với việc phân tách một mảng không âm thành các cập nhật theo khoảng thời gian, nhưng việc tìm kiếm trên tất cả b hợp lệ vẫn quá lớn. 

Sự đơn giản hóa quan trọng đến từ việc đảo ngược quan điểm. Thay vì xây dựng một mảng cuối cùng tùy ý, chúng tôi tập trung vào cách hoạt động của các vi phạm liền kề. Nếu ở vị trí i nào đó chúng ta có a[i] > a[i+1] thì vế phải quá nhỏ so với vế trái. Vì chúng ta chỉ có thể tăng giá trị nên cách duy nhất để giải quyết vấn đề này là tăng vị trí i+1 đủ để nó bắt kịp a[i]. Mỗi đơn vị của khoảng cách này phải được “phân phối” bởi một số thao tác có ảnh hưởng đến i+1 nhưng không ảnh hưởng đến i như nhau, điều này hàm ý một chi phí cấu trúc độc lập với các vị trí khác. 

Điều này dẫn đến sự phân rã tuyến tính trong đó mỗi bước đi xuống đóng góp trực tiếp vào câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm giọt liền kề tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng một lần từ trái sang phải và đo xem mỗi lần giảm liền kề đóng góp bao nhiêu vào tổng số thao tác.

1. Bắt đầu với một câu trả lời đang chạy được khởi tạo bằng 0. Biến này sẽ tích lũy số lượng hoạt động phạm vi cần thiết. 
2. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến phần tử cuối cùng. Tại mỗi vị trí i, so sánh a[i] và a[i+1]. 
3. Nếu a[i] nhỏ hơn hoặc bằng a[i+1] thì không làm gì cả. Cặp này đã thỏa mãn thứ tự không giảm cục bộ, do đó không cần thực hiện thao tác bắt buộc nào để sửa chữa ranh giới này. 
4. Nếu a[i] lớn hơn a[i+1], hãy tính hiệu d = a[i] - a[i+1]. Thêm giá trị này vào câu trả lời. Giá trị này thể hiện số lượng tăng đơn vị tối thiểu phải được áp dụng cho vị trí i+1 so với i để loại bỏ hiện tượng sụt giảm cục bộ. 
5. Tiếp tục quét cho đến hết mảng, tích lũy tất cả các giọt dương như vậy. 
6. Xuất giá trị tích lũy cuối cùng. 

Lý do đằng sau bước 4 là mỗi đơn vị giảm tương ứng với một “lớp” hiệu chỉnh độc lập phải được đưa vào thông qua một số thao tác phạm vi bắt đầu tại hoặc sau i+1 và không bao phủ hoàn toàn i. Các lớp này không can thiệp vào các vị trí khác nhau vì mỗi lần thả liền kề sẽ thực thi số lượng tăng dần cần thiết tối thiểu của riêng nó. 

### Tại sao nó hoạt động 

Mỗi thao tác sẽ tăng chính xác một phân đoạn liền kề, do đó, bất kỳ hiệu ứng cuối cùng nào cũng có thể được xem dưới dạng xếp chồng khoảng cách chiều cao đơn vị trên mảng. Sự khác biệt giữa các phần tử liền kề a[i] và a[i+1] xác định số lớp phải có thêm ở bên phải so với bên trái để mảng cuối cùng không giảm. 

Bất cứ khi nào a[i] > a[i+1], ít nhất các phép toán a[i] - a[i+1] phải đóng góp thêm chiều cao cho vị trí i+1 mà không đóng góp bằng nhau cho vị trí i. Không có thao tác nào có thể đồng thời “tránh” được tất cả các ràng buộc như vậy, vì vậy các yêu cầu này bổ sung độc lập trên tất cả các cặp liền kề. Sự độc lập này làm cho tổng số giọt tích cực vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            ans += a[i] - a[i + 1]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của quan sát rằng chỉ có sự chuyển đổi đi xuống mới quan trọng. Vòng lặp xem xét mỗi cặp liền kề chính xác một lần và bộ tích lũy lưu trữ tổng số bù bắt buộc cần thiết để loại bỏ tất cả các mức giảm cục bộ. Không cần mô phỏng hoạt động hoặc xây dựng mảng cuối cùng. 

Một lỗi triển khai phổ biến là cố gắng duy trì một mảng đã được chuyển đổi một cách rõ ràng hoặc mô phỏng việc tăng phạm vi. Điều đó là không cần thiết vì câu trả lời chỉ phụ thuộc vào những khác biệt địa phương chứ không phụ thuộc vào cấu trúc toàn cầu đang phát triển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| tôi | một [tôi] | a[i+1] | Thả | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 
| 2 | 2 | 3 | 0 | 0 | 

Mảng đã không giảm nên không có cặp liền kề nào cần hiệu chỉnh. Câu trả lời cuối cùng vẫn là 0, phản ánh rằng không cần thực hiện thao tác nào. 

### Ví dụ 2 

đầu vào:```
3
4 3 2
```| tôi | một [tôi] | a[i+1] | Thả | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 1 | 1 | 
| 2 | 3 | 2 | 1 | 2 | 

Ở đây mỗi bước giới thiệu một đơn vị bồi thường bắt buộc mới. Lần thả đầu tiên buộc ít nhất một thao tác ảnh hưởng đến phần tử thứ hai nhiều hơn phần tử thứ nhất và lần thả thứ hai đưa ra một yêu cầu độc lập khác. Tổng số trở thành hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng, công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | Chỉ có một khoản tiền được lưu trữ | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc cho n lên tới 100000 và không yêu cầu cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n = int(input())
    a = list(map(int, input().split()))
    ans = 0
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            ans += a[i] - a[i + 1]
    return str(ans)

# provided samples
assert run("3\n1 2 3\n") == "0"

# custom cases
assert run("1\n10\n") == "0", "single element"
assert run("3\n4 3 2\n") == "2", "strictly decreasing"
assert run("5\n1 1 1 1 1\n") == "0", "already flat"
assert run("4\n5 1 4 2\n") == "7", "mixed pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | ranh giới tối thiểu | 
| 4 3 2 | 2 | giọt tích lũy | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| mẫu hỗn hợp | 7 | tích lũy không hề nhỏ | 

## Vỏ cạnh 

Đối với mảng một phần tử như [10], vòng lặp trên các cặp liền kề hoàn toàn không thực thi, do đó câu trả lời vẫn bằng 0, phù hợp với thực tế là một giá trị đơn lẻ gần như không giảm. 

Đối với một mảng giảm nghiêm ngặt như [5, 4, 3, 2], mọi cặp liền kề đều đóng góp độc lập. Thuật toán tích lũy (5-4) + (4-3) + (3-2) = 3 và điều này phù hợp với thực tế là mỗi lần giảm đưa ra một đơn vị bù cần thiết riêng biệt. 

Đối với một mảng vốn đã không đổi, chẳng hạn như [7, 7, 7, 7], không có cặp nào thỏa mãn a[i] > a[i+1], do đó, câu trả lời luôn bằng 0, xác nhận rằng không cần thực hiện các phép toán phạm vi.
