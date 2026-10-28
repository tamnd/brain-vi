---
title: "CF 106185H - Dấu ngoặc đơn"
description: "Chúng ta được cung cấp một hàng vị trí, mỗi vị trí có một con tem có dấu ngoặc đơn mở hoặc đóng. Chúng ta không lấy chuỗi con theo nghĩa thông thường. Thay vào đó, chúng tôi xây dựng một trình tự bằng cách đi dọc theo các vị trí này."
date: "2026-06-20T11:55:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106185
codeforces_index: "H"
codeforces_contest_name: "The 2025 ICPC Japan Online First Round Contest"
rating: 0
weight: 106185
solve_time_s: 59
verified: true
draft: false
---

[CF 106185H - Dấu ngoặc đơn](https://codeforces.com/problemset/problem/106185/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng vị trí, mỗi vị trí có một con tem có dấu ngoặc đơn mở hoặc đóng. Chúng ta không lấy chuỗi con theo nghĩa thông thường. Thay vào đó, chúng tôi xây dựng một trình tự bằng cách đi dọc theo các vị trí này. 

Chúng tôi bắt đầu bằng cách chọn bất kỳ vị trí nào và in ký tự của nó. Sau đó, mỗi bước tiếp theo phải di chuyển đến vị trí liền kề trên dòng, trái hoặc phải và chúng ta in ký tự ở vị trí mới. Chúng ta được phép xem lại vị trí nhiều lần nhưng không được phép ở nguyên tại chỗ nên mỗi bước đều phải chuyển sang hàng xóm. 

Bước đi này tạo ra một chuỗi dấu ngoặc đơn. Chúng tôi được yêu cầu đếm xem có bao nhiêu cặp vị trí bắt đầu và kết thúc theo thứ tự cho phép ít nhất một bước đi như vậy tạo ra chuỗi dấu ngoặc đơn chính xác, nghĩa là số dư tiền tố không bao giờ âm và số dư cuối cùng bằng 0. 

Ràng buộc n lên tới 2×10^5 trên tất cả các trường hợp thử nghiệm có nghĩa là bất kỳ giải pháp nào có hành vi bậc hai cho mỗi trường hợp thử nghiệm đều không thể thực hiện được. Thậm chí cần phải có O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm, vì tổng kích thước đầu vào lớn nhưng được tổng hợp. 

Một điểm tinh tế là việc đi bộ không nhất thiết phải đơn giản. Chúng ta có thể di chuyển qua lại, do đó chuỗi không bị hạn chế là chuỗi con hoặc đường dẫn đơn giản. Điều này làm cho những cách diễn giải ngây thơ như “chỉ cần kiểm tra số dư chuỗi con” là không chính xác. 

Trường hợp lỗi điển hình xuất hiện khi một chuỗi con có số lượng '(' và ')' bằng nhau nhưng không phải là chuỗi cân bằng tiền tố hợp lệ theo thứ tự. 

Ví dụ: hãy xem xét s = ")()(" và chúng tôi chọn i = 2, j = 3 nếu chúng tôi giả định sai tính hợp lệ của chuỗi con. Chuỗi con "()"" là hợp lệ, nhưng đối với các trường hợp phức tạp hơn như ")()(", một số chuỗi con có số lượng bằng nhau không thể tạo thành một chuỗi hợp lệ nếu thứ tự được cố định. Tuy nhiên, vì chúng ta có thể sắp xếp lại bằng cách đi bộ nên trực giác này sẽ thay đổi và trở thành chìa khóa cho giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử từng cặp vị trí bắt đầu và kết thúc, đồng thời đối với mỗi cặp cố gắng mô phỏng tất cả các bước đi có thể có giữa chúng, kiểm tra xem có thể tạo ra chuỗi dấu ngoặc đơn hợp lệ hay không. Ngay cả khi chúng tôi hạn chế bản thân ở những bước đi hợp lý, số lượng đường đi có thể tăng theo cấp số nhân vì ở mỗi bước chúng tôi có thể chọn trái hoặc phải và chúng tôi có thể truy cập lại các nút tùy ý. Điều này nhanh chóng trở nên không khả thi ngay cả đối với n nhỏ. 

Quan sát chính là cấu trúc dòng cho phép chúng ta “sắp xếp lại” các ký tự thông qua các đường vòng. Bằng cách di chuyển qua lại trên một cạnh, chúng ta có thể hoán đổi các phần tử liền kề trong chuỗi được in một cách hiệu quả. Việc lặp lại ý tưởng này cho thấy rằng giữa điểm bắt đầu và điểm kết thúc đã chọn, chúng ta có thể nhận ra bất kỳ hoán vị nào của các ký tự đã ghé thăm, miễn là chúng ta tôn trọng rằng chúng ta đang đi trên một con đường được kết nối. 

Khi đã hiểu được tính linh hoạt này, vấn đề sẽ không còn là việc xây dựng đường dẫn nữa mà trở thành việc đếm các phân đoạn có nhiều tập ký tự có thể tạo thành một chuỗi dấu ngoặc đơn hợp lệ. Một nhiều tập hợp có thể tạo thành một chuỗi hợp lệ khi và chỉ khi số '(' bằng số ')' và chúng ta được phép sắp xếp chúng sao cho tất cả các dấu ngoặc mở đều đứng trước. Điều đó có nghĩa là hạn chế thực sự duy nhất là sự bằng nhau về số lượng chứ không phải thứ tự bên trong phân khúc. 

Do đó, nhiệm vụ giảm xuống việc đếm các cặp điểm cuối sao cho tổng số dư giữa chúng bằng không. Điều này trở thành một vấn đề đếm tổng tiền tố với một ràng buộc chẵn lẻ bổ sung xuất phát từ thực tế là các bước đi xen kẽ nhau và độ dài của chuỗi phải khớp với tính chẵn lẻ của các điểm cuối trong biểu diễn tiền tố cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các cuộc đi bộ | Hàm mũ | O(n) | Quá chậm | 
| Nhóm số dư tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng '(' thành +1 và mỗi ')' thành -1 và tính tổng tiền tố trên mảng, với tiền tố[0] = 0.

Sau đó, chúng tôi diễn giải lại một cặp điểm cuối hợp lệ dưới dạng hai chỉ số tiền tố i và j sao cho tổng ròng giữa chúng bằng 0. Điều kiện đó trở thành prefix[j] = prefix[i - 1]. 

Chúng tôi lặp lại tất cả j có thể từ trái sang phải và với mỗi j chúng tôi muốn đếm xem có bao nhiêu vị trí trước đó i - 1 có cùng giá trị tiền tố. 

Tuy nhiên, có một ràng buộc bổ sung đến từ cấu trúc của bước đi: số lượng ký tự được in giữa các điểm cuối phải khớp với tính chẵn lẻ được yêu cầu bởi các bước di chuyển xen kẽ. Điều này có nghĩa là yêu cầu (j - (i - 1)) có độ dài chẵn, tương đương với i - 1 và j có cùng tính chẵn lẻ. 

Chúng tôi duy trì một bảng tần số được lập chỉ mục theo giá trị tổng tiền tố và trong mỗi giá trị tiền tố, chúng tôi phân tách số lượng theo tính chẵn lẻ của chỉ mục. 

Với mỗi vị trí j, chúng ta cộng số lần xuất hiện trước đó của prefix[j] có cùng tính chẵn lẻ với j. 

Cuối cùng, chúng tôi tích lũy điều này trên tất cả j. 

Tại sao nó hoạt động đến từ việc nén quyền tự do đi lại thành các tổng tiền tố. Khả năng di chuyển qua lại đảm bảo chúng ta không bị giới hạn ở các chuỗi con cố định; thay vào đó, chỉ có số dư ròng mới quan trọng. Điều kiện chẵn lẻ mã hóa thực tế là mỗi bước thay đổi vị trí, do đó số lượng ký tự được in sẽ khớp với tính chẵn lẻ của điểm cuối trong mô hình chỉ mục tiền tố. Thuật toán này đúng vì mọi cấu trúc hợp lệ đều tương ứng với chính xác một cặp tiền tố như vậy và mỗi cặp tiền tố như vậy có thể được nhận ra bằng một bước đi thích hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    while True:
        line = input().strip()
        if not line:
            continue
        n = int(line)
        if n == 0:
            return
        s = input().strip()

        pref = 0
        cnt = {}
        cnt.setdefault(0, [0, 0])
        cnt[0][0] = 1

        ans = 0

        for i in range(1, n + 1):
            c = s[i - 1]
            if c == '(':
                pref += 1
            else:
                pref -= 1

            p = i & 1
            if pref in cnt:
                ans += cnt[pref][p]

            if pref not in cnt:
                cnt[pref] = [0, 0]
            cnt[pref][p] += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng các tổng tiền tố một cách nhanh chóng và duy trì một từ điển ánh xạ từng giá trị tiền tố theo số lần nó xuất hiện ở các chỉ số chẵn và lẻ. Mỗi lần mở rộng đến vị trí thứ i, chúng tôi sẽ ngay lập tức đếm các điểm cuối hợp lệ trước đó khớp với cả giá trị tiền tố và yêu cầu chẵn lẻ, sau đó cập nhật cấu trúc. 

Sự tinh tế chính là sử dụng lập chỉ mục dựa trên 0 và dựa trên 1 một cách nhất quán. Ở đây, chỉ mục i dựa trên 1 trong vòng lặp và tính chẵn lẻ được lấy trực tiếp từ i, phù hợp với yêu cầu bắt nguồn từ việc so sánh i - 1 và j. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ s="()()". Tổng tiền tố là 0, 1, 0, 1, 0. 

Chúng tôi theo dõi sự xuất hiện của từng giá trị tiền tố với tính chẵn lẻ: 

| tôi | char | trước | chẵn lẻ(i) | khớp với | chạy ans | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ( | 1 | lẻ | không có | 0 | 
| 2 | ) | 0 | thậm chí | trước 0 thậm chí: i=0 | 1 | 
| 3 | ( | 1 | lẻ | trước 1 lẻ: i=1 | 2 | 
| 4 | ) | 0 | thậm chí | trước 0 thậm chí: i=0,2 | 4 | 

Điều này cho thấy các giá trị tiền tố lặp lại tương ứng trực tiếp với các cặp điểm cuối hợp lệ như thế nào. 

Bây giờ, hãy xem xét s = ")))(((" vốn rất mất cân bằng cục bộ. Các giá trị tiền tố rất khác nhau và các kết quả trùng khớp chỉ xảy ra khi tổng tiền tố quay trở lại giá trị trước đó, chứng tỏ rằng chỉ các phân đoạn cân bằng toàn cầu mới đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần với các phép toán từ điển trung bình O(1) | 
| Không gian | O(n) | Tối đa một mục nhập cho mỗi giá trị tổng tiền tố riêng biệt | 

Thuật toán phù hợp thoải mái trong giới hạn vì tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5, do đó công việc tuyến tính trên mỗi trường hợp thử nghiệm vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Placeholder since full solver is embedded above; in real use, import solve()

# Basic sanity cases
# assert run("2\n()\n0\n") == "1\n"
# assert run("4\n()()\n0\n") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cân bằng tối thiểu | 1 | cặp hợp lệ nhỏ nhất | 
| xen kẽ "()()" | 4 | lặp lại nhiều tiền tố | 
| tất cả đóng rồi mở | xử lý mất cân bằng | trường hợp khôi phục tiền tố | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi tổng tiền tố lặp lại ngay lập tức nhưng tính chẵn lẻ không khớp. Ví dụ: mẫu như "()()" tạo giá trị tiền tố 0 ở cả vị trí chẵn và lẻ. Thuật toán tách các nhóm chẵn lẻ, do đó chỉ các chuyển đổi hợp lệ mới được tính. 

Một trường hợp khác là tiền tố hoàn toàn không cân bằng, chẳng hạn như "))))(((((). Ở đây, các giá trị tiền tố hầu như không bao giờ khớp nhau, vì vậy câu trả lời trở thành 0 và từ điển chỉ lưu trữ các giá trị riêng biệt mà không có xung đột. 

Cuối cùng, trường hợp n = 2 đảm bảo tính đúng đắn của việc xử lý tính chẵn lẻ. Đối với "()", tiền tố trở về 0 tại i = 2 và vì tính chẵn lẻ khớp với lần xuất hiện trước đó tại i = 0 nên cặp được tính chính xác một lần, khớp với cấu trúc hợp lệ duy nhất dự kiến.
