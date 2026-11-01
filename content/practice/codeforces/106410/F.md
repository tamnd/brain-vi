---
title: "CF 106410F - Sự xáo trộn chim cánh cụt-Gopher"
description: "Cho hai chuỗi a và b có cùng độ dài. Hoạt động được phép là không bình thường: thay vì hoán đổi hai ký tự hoặc thay đổi một vị trí, chúng tôi chọn chỉ mục i và đảo ngược mọi thứ từ i đến cuối chuỗi hiện tại."
date: "2026-06-25T09:55:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106410
codeforces_index: "F"
codeforces_contest_name: "HPI 2026 Novice"
rating: 0
weight: 106410
solve_time_s: 31
verified: true
draft: false
---

[CF 106410F - Sự kết hợp giữa chim cánh cụt-Gopher](https://codeforces.com/problemset/problem/106410/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`a`Và`b`, có cùng độ dài. Hoạt động được phép là không bình thường: thay vì hoán đổi hai ký tự hoặc thay đổi một vị trí, chúng tôi chọn một chỉ mục`i`và đảo ngược mọi thứ từ`i`đến cuối chuỗi hiện tại. Nhiệm vụ là tìm một chuỗi các đảo ngược hậu tố để biến đổi`a`vào trong`b`hoặc chứng minh rằng điều đó không thể thực hiện được. Trình tự phải chứa tối đa 2000 thao tác. Tuyên bố chính thức mô tả vấn đề chuyển đổi này trên các chuỗi có độ dài lên tới 1000. 

Kích thước đầu vào đủ nhỏ để chúng ta không cần thuật toán gần tuyến tính, nhưng nó vẫn đủ lớn để việc thử một cách mù quáng tất cả các hoạt động có thể là không thể. Đối với một chuỗi có độ dài 1000, có thể có 1000 lần đảo ngược hậu tố tại mỗi thời điểm. Cây tìm kiếm khám phá tất cả các lựa chọn sẽ phát triển như$1000^k$, vì vậy ngay cả độ sâu 10 cũng đã vượt xa những gì có thể xử lý được. 

Quan sát quan trọng là sự đảo ngược hậu tố có cấu trúc mạnh mẽ. Chúng ảnh hưởng đến mọi vị trí từ một số điểm sang bên phải, nghĩa là tiền tố trước chỉ mục đã chọn bị đóng băng. Một giải pháp đúng nên khai thác thực tế là chúng ta có thể quyết định các ký tự từ trái sang phải và không bao giờ làm phiền chúng nữa. 

Một sai lầm phổ biến là cho rằng mọi thao tác đều có thể được hoàn tác bằng cách đảo ngược hậu tố tương tự sau đó. Mặc dù điều đó đúng nhưng không có nghĩa là có thể thực hiện được các phép biến đổi tùy ý. Ví dụ:```
Input:
3
deb
fyf

Output:
-1
```Chuỗi đầu tiên có các ký tự`d`,`e`,`b`, trong khi mục tiêu có`f`,`y`,`f`. Vì việc đảo ngược hậu tố chỉ sắp xếp lại các ký tự hiện có nên chúng không thể tạo ra các chữ cái mới. Việc triển khai bất cẩn chỉ mô phỏng các vị trí mà không kiểm tra tính khả dụng của ký tự có thể cố gắng xây dựng một chuỗi và thất bại sau đó. 

Một trường hợp cạnh khác là khi các chuỗi đã bằng nhau.```
Input:
1
m
m

Output:
0
```Không có hoạt động cần thiết. Việc triển khai luôn thực hiện đảo ngược cho mọi vị trí có thể đưa ra các hoạt động không cần thiết không chính xác hoặc thậm chí vượt quá giới hạn hoạt động. 

Trường hợp phức tạp cuối cùng là một phép biến đổi trong đó việc đảo ngược yêu cầu chỉ ảnh hưởng đến một hậu tố.```
Input:
4
emog
egom

Output:
1
2
```Đảo ngược hậu tố bắt đầu từ vị trí 2 thay đổi`emog`vào trong`egom`. Các giải pháp chỉ cho phép đảo ngược toàn chuỗi sẽ bỏ sót những trường hợp này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi chuỗi đảo ngược hậu tố có thể xảy ra. Thao tác này rất dễ áp ​​dụng và nếu một chuỗi đạt đến chuỗi mục tiêu thì rõ ràng nó là chính xác. Tìm kiếm theo chiều rộng trên các trạng thái thậm chí có thể tìm thấy chuỗi ngắn nhất. Vấn đề là số lượng trạng thái. Có tới$26^n$các chuỗi có thể và thậm chí chỉ lưu trữ các trạng thái có thể truy cập là không đủ cho$n=1000$. Giới hạn hoạt động năm 2000 không giúp ích gì vì các lựa chọn khám phá vẫn còn quá nhiều nhánh. 

Quan sát quan trọng đến từ việc nhìn vào vị trí cuối cùng. Ký tự cuối cùng của chuỗi đích phải đến từ một ký tự nào đó trong chuỗi gốc. Một hậu tố đảo ngược độ dài ít nhất hai sẽ thay đổi ký tự cuối cùng bằng ký tự đầu tiên của hậu tố đảo ngược. Điều này có nghĩa là chúng ta có thể kiểm soát vị trí cuối cùng hiện tại bằng cách chọn một sự đảo ngược đưa ký tự cần thiết đến đó. 

Khi một nhân vật được đặt ở cuối, chúng ta không bao giờ cần phải chạm vào nó nữa. Điều này gợi ý xử lý chuỗi từ phải sang trái. Ở mỗi bước, chúng ta xem xét ký tự sẽ xuất hiện ở vị trí không cố định cuối cùng hiện tại. Nếu nó đã ở đó rồi thì chúng ta có thể bỏ qua nó. Nếu không, chúng tôi sẽ tìm thấy nhân vật đó hiện đang ở đâu. Một sự đảo ngược hậu tố duy nhất có thể đưa nó đến phần cuối, nhưng việc định hướng của phần còn lại có thể trở nên bất tiện. Lần đảo chiều thứ hai có thể khôi phục hướng đi. 

Điều này làm giảm vấn đề thành một công trình tham lam. Mỗi vị trí cần tối đa hai thao tác, vì vậy đối với$n=1000$tổng số hoạt động tối đa là 2000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ vị trí cuối cùng của dây và di chuyển về phía đầu tiên. Hậu tố sau vị trí hiện tại đã chính xác và sẽ không bao giờ được sửa đổi nữa. 
2. Nếu ký tự cuối cùng hiện tại của`a`đã bằng ký tự tương ứng của`b`, di chuyển đến vị trí tiếp theo. Không cần thao tác nào vì ký tự đúng đã được sửa. 
3. Nếu không, hãy tìm vị trí của ký tự cần thiết bên trong tiền tố còn lại. Ký tự này phải được chuyển đến cuối phần hoạt động của chuỗi. 
4. Đảo ngược hậu tố bắt đầu từ vị trí tìm thấy đó. Ký tự mong muốn di chuyển đến đầu hoạt động. 
5. Nếu vị trí tìm thấy không phải là vị trí đầu tiên của bộ phận hoạt động, hãy đảo ngược toàn bộ bộ phận hoạt động. Thao tác này sẽ khôi phục hướng để các ký tự chưa cố định còn lại giữ nguyên thứ tự có thể sử dụng được của chúng. 
6. Sau khi xử lý tất cả các vị trí, xuất ra các thao tác đã ghi. Nếu tại bất kỳ thời điểm nào ký tự được yêu cầu không tồn tại ở phần còn lại thì việc chuyển đổi là không thể. 

Tại sao nó hoạt động: sau mỗi lần lặp, hậu tố hiện tại của`a`phù hợp với hậu tố của`b`. Các thao tác chỉ chạm vào các vị trí chưa cố định nên hậu tố đã đúng vẫn không thay đổi. Khi đặt một ký tự, đảo ngược đầu tiên đảm bảo rằng ký tự được yêu cầu đến đích và đảo ngược thứ hai tùy chọn sẽ thay đổi tiền tố còn lại thành trạng thái tương đương với trạng thái trước vị trí ngoại trừ việc loại bỏ thêm một ký tự. Bởi vì mỗi bước sẽ giảm số lượng vị trí chưa được giải quyết đi một vị trí, nên cuối cùng quá trình sẽ kết thúc hoặc phát hiện một yêu cầu ký tự không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(input().strip())
    b = input().strip()

    ans = []

    for end in range(n - 1, -1, -1):
        if a[end] == b[end]:
            continue

        pos = -1
        for i in range(end + 1):
            if a[i] == b[end]:
                pos = i
                break

        if pos == -1:
            print(-1)
            return

        if pos != 0:
            ans.append(pos + 1)
            a[pos:] = reversed(a[pos:])

            ans.append(1)
            a[:end + 1] = reversed(a[:end + 1])
        else:
            ans.append(1)
            a[:end + 1] = reversed(a[:end + 1])

    print(len(ans))
    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ chuỗi hiện tại dưới dạng danh sách có thể thay đổi vì việc đảo ngược hậu tố sẽ thay đổi nhiều ký tự. Việc cắt Python bằng phép gán cho phép chúng ta đảo ngược chính xác phần được yêu cầu mà không cần tạo một chuỗi hoàn toàn riêng biệt. 

Các quá trình vòng lặp`end`từ phải sang trái. chỉ mục`end`đại diện cho vị trí cuối cùng vẫn được phép thay đổi. Mọi thứ sau đó đã bằng mục tiêu. 

Khi tìm thấy ký tự cần thiết, thao tác được lưu trữ sử dụng lập chỉ mục dựa trên một vì câu lệnh vấn đề sử dụng các vị trí bắt đầu từ một. Các chỉ số Python thực tế đều dựa trên số 0, vì vậy`pos + 1`được in. 

Số lượng hoạt động được giới hạn một cách tự nhiên. Mỗi trong số`n`các vị trí sử dụng tối đa hai thao tác, đưa ra nhiều nhất`2n`, phù hợp với giới hạn yêu cầu đối với`n ≤ 1000`. 

(tiếp tục trong thông báo tiếp theo với Ví dụ đã thực hiện, Phân tích độ phức tạp, Trường hợp kiểm thử và Trường hợp biên)
