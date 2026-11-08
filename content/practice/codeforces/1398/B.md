---
title: "CF 1398B - Trò chơi loại bỏ chuỗi con"
description: "Trò chơi hoạt động trên một chuỗi nhị phân có thể được coi là một chuỗi các khối liền kề có các ký tự giống hệt nhau. Ở mỗi lượt, người chơi loại bỏ một đoạn liền kề gồm các ký tự giống nhau và hai phần còn lại của chuỗi được khâu lại với nhau."
date: "2026-06-11T09:07:30+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1398
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 93 (Rated for Div. 2)"
rating: 800
weight: 1398
solve_time_s: 78
verified: true
draft: false
---

[CF 1398B - Trò chơi loại bỏ chuỗi con](https://codeforces.com/problemset/problem/1398/B) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam, phân loại 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi hoạt động trên một chuỗi nhị phân có thể được coi là một chuỗi các khối liền kề có các ký tự giống hệt nhau. Ở mỗi lượt, người chơi loại bỏ một đoạn liền kề gồm các ký tự giống nhau và hai phần còn lại của chuỗi được khâu lại với nhau. Điểm số không phụ thuộc vào việc người chơi thực hiện bao nhiêu bước di chuyển mà cụ thể là bao nhiêu bước`1`các ký tự mà họ đích thân xóa trong toàn bộ quá trình. 

Điều tinh tế quan trọng là một bước di chuyển không bị hạn chế trong việc loại bỏ một ký tự đơn lẻ. Người chơi có thể loại bỏ toàn bộ các chuỗi ký tự giống hệt nhau chỉ bằng một hành động, điều đó có nghĩa là về cơ bản, trò chơi là về việc chọn các khối cần loại bỏ và theo thứ tự nào, trong khi cấu trúc của các chuỗi còn lại liên tục thay đổi khi các khối hợp nhất. 

Kích thước đầu vào nhỏ cho mỗi trường hợp thử nghiệm, với các chuỗi có độ dài tối đa là 100 và tối đa 500 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng trạng thái trò chơi theo cấp số nhân. Ngay cả một mô phỏng bậc ba hoặc bậc hai trên tất cả các chuỗi con cũng có thể được chấp nhận trong mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì khám phá toàn bộ cây trò chơi đều là quá mức cần thiết không cần thiết. 

Một sai lầm ngây thơ nảy sinh khi coi đây là “đếm những cái mà Alice có thể lấy trong các khối xen kẽ”. Điều đó không thành công vì việc xóa có thể hợp nhất các phần riêng biệt của chuỗi, tạo ra vùng lân cận mới và cơ hội mới. 

Ví dụ, hãy xem xét`101`. Nếu Alice tham lam loại bỏ cái đầu tiên`1`, chuỗi trở thành`01`, nhưng nếu thay vào đó cô ấy loại bỏ phần giữa`0`, chuỗi trở thành`11`, thay đổi đáng kể cơ hội ghi bàn trong tương lai. Một chiến lược bỏ qua những thay đổi về cấu trúc sẽ tính toán sai ngay cả với những đầu vào nhỏ như vậy. 

Một trường hợp thất bại khác là các chuỗi xen kẽ như`101010`. Cách chơi tốt nhất phụ thuộc vào việc nhóm các hiệu ứng sau khi loại bỏ và việc loại bỏ các khối hiển thị một cách ngây thơ và tham lam không bảo toàn được cấu trúc tối ưu. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là mô phỏng toàn bộ trò chơi dưới dạng tìm kiếm tối đa. Mỗi trạng thái là một chuỗi và mỗi lần di chuyển sẽ chọn một chuỗi ký tự giống hệt nhau để xóa. Số lần di chuyển có thể có từ một trạng thái tỷ lệ thuận với số lần chạy và sau mỗi lần di chuyển, chuỗi sẽ thay đổi, tạo ra một trạng thái mới. Điều này nhanh chóng trở thành một cây trò chơi đầy đủ. 

Mặc dù độ dài chuỗi tối đa là 100, nhưng hệ số phân nhánh là khoảng O(n) mỗi lần di chuyển và độ sâu cũng là O(n), dẫn đến số lượng trạng thái lớn về mặt thiên văn. Về nguyên tắc, việc ghi nhớ giúp ích, nhưng số lượng trạng thái riêng biệt vẫn theo cấp số nhân vì việc xóa có thể sắp xếp lại các hoạt động kề và hợp nhất theo nhiều cách. 

Quan sát quan trọng là danh tính của từng ký tự không quan trọng bằng cấu trúc của các lần chạy liên tiếp. Khi chúng ta nén chuỗi thành các khối, trò chơi sẽ trở thành khoảng bao nhiêu`1`-các khối tồn tại và cách chúng tương tác với xung quanh`0`-khối. Mỗi nước đi luôn loại bỏ toàn bộ một khối và điều quan trọng duy nhất là liệu Alice hay Bob có được một khối hay không.`1`-block trước khi nó không thể truy cập được hoặc được hợp nhất trong một cấu hình kém thuận lợi hơn. 

Một cái nhìn đơn giản và sâu sắc hơn là tất cả`1`các khối cuối cùng sẽ bị ai đó xóa và người chơi thay phiên nhau lấy toàn bộ khối. Vì mỗi lần di chuyển sẽ loại bỏ chính xác một khối liền kề nên trò chơi chuyển sang chọn các khối theo thứ tự vị trí giảm dần sau khi hiệu ứng nén được giải quyết và cách chơi tối ưu sẽ phù hợp với việc xử lý độ dài khối theo thứ tự được sắp xếp. 

Sau khi chuỗi được nén thành các lần chạy, quy trình sẽ trở thành một lựa chọn trọng số phân đoạn theo lượt một cách hiệu quả, trong đó mỗi chuỗi`1`-run đóng góp độ dài của nó cho người chơi nhận được nó và cả hai người chơi luân phiên chọn từ những đóng góp lớn nhất hiện có một cách tham lam. 

Điều này làm giảm vấn đề từ thao tác chuỗi động sang trò chơi nhiều tập tĩnh trong suốt thời gian chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cây trò chơi) | Hàm mũ | Hàm mũ | Quá chậm | 
| Chạy nén + lựa chọn tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên nén chuỗi thành các đoạn liên tiếp tối đa. Mỗi đoạn được ghi lại dưới dạng một cặp bao gồm ký tự và độ dài của nó. Điều này là cần thiết vì mỗi lần di chuyển sẽ xóa toàn bộ khối bằng nhau liền kề, do đó đơn vị nguyên tử tự nhiên của trò chơi là một lần chạy chứ không phải một nhân vật. 
2. Chỉ trích xuất độ dài của các đoạn chứa`1`. Chúng đại diện cho tất cả các nguồn có thể có của điểm của Alice, vì chỉ việc xóa`1`đóng góp vào điểm số của cô ấy. 
3. Sắp xếp chúng`1`- Độ dài đoạn theo thứ tự giảm dần Lý do là cả hai người chơi đều đang cạnh tranh hiệu quả để sớm chiếm được các khối ghi điểm lớn, vì việc trì hoãn một khối lớn chỉ cho phép đối thủ chiếm được nó ở vị trí tương đương hoặc thuận lợi hơn. 
4. Mô phỏng luân phiên lần lượt danh sách đã sắp xếp này. Alice lấy thứ nhất, thứ ba, thứ năm, v.v. lớn nhất có sẵn`1`-phân đoạn, trong khi Bob lấy phân đoạn thứ hai, thứ tư, v.v. 
5. Tính tổng các giá trị Alice lấy để đưa ra câu trả lời cuối cùng. 

Bước suy luận quan trọng là khi chuỗi được giảm xuống thành các khối độc lập, sự tương tác giữa các khối ở xa không còn thay đổi kích thước đóng góp của chúng nữa. Sự phức tạp của trò chơi trở thành một vấn đề phân bổ mang tính cạnh tranh đơn giản trên các trọng số độc lập. 

### Tại sao nó hoạt động 

Mỗi lần di chuyển sẽ xóa chính xác một khối liền kề tối đa. Sau khi nén hoàn toàn, quyết định có ý nghĩa duy nhất là khối nào sẽ bị xóa trước đó. Vì việc sáp nhập không bao giờ làm tăng tổng số`1`s trong một khối, mỗi khối`1`-đoạn giữ nguyên trọng lượng của nó cho đến khi bị loại bỏ. Thứ tự lần lượt xen kẽ cố định quyền sở hữu các trọng số này theo thứ tự giảm dần và không việc sắp xếp lại các bước di chuyển có thể cải thiện khả năng truy cập của Alice vào một khối nhỏ hơn mà không cấp cho Bob quyền truy cập vào khối bằng hoặc lớn hơn trước đó. Điều này khóa chiến lược tối ưu vào việc lựa chọn tham lam trên các trọng số phân khúc được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        s = input().strip()
        
        ones = []
        
        i = 0
        n = len(s)
        
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            
            if s[i] == '1':
                ones.append(j - i)
            
            i = j
        
        ones.sort(reverse=True)
        
        ans = 0
        for k in range(0, len(ones), 2):
            ans += ones[k]
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách quét chuỗi và xây dựng độ dài chạy. Vòng lặp bên trong xác định các phân đoạn liền kề tối đa, đảm bảo mỗi khối được xử lý nguyên tử. Chỉ các phân đoạn của những phân đoạn đó được lưu trữ vì các phân đoạn bằng 0 không đóng góp vào điểm của Alice. 

Việc sắp xếp các độ dài phân đoạn này theo thứ tự giảm dần phù hợp với chiến lược phân bổ tham lam được đưa ra trước đó. Vòng lặp tính tổng mọi phần tử thứ hai bắt đầu từ chỉ số 0 tương ứng trực tiếp với việc Alice lấy các khối có sẵn tốt nhất thứ nhất, thứ ba và tiếp theo. 

Một cạm bẫy triển khai phổ biến là quên nén các lần chạy và thay vào đó đếm các ký tự riêng lẻ, điều này phá vỡ giả định rằng các bước di chuyển hoạt động trên toàn bộ khối. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`01111001`. Bước nén tạo ra các phân đoạn`0 | 1111 | 00 | 1`. các`1`-các đoạn có độ dài`[4, 1]`. 

| Bước | Những phân khúc | Đã sắp xếp | Alice chọn | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [4, 1] | [4, 1] | 4 | 4 | 
| Tiếp theo | [4, 1] | [4, 1] | 1 (Bob mất 1) | 4 | 

Điểm của Alice trở thành 4. 

Bây giờ hãy xem xét`101010`. Nén mang lại`1 | 0 | 1 | 0 | 1 | 0`, vậy những phân đoạn đó là`[1, 1, 1]`. 

| Bước | Những phân khúc | Đã sắp xếp | Alice chọn | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [1, 1, 1] | [1, 1, 1] | 1 | 1 | 
| Tiếp theo | [1, 1, 1] | [1, 1, 1] | 1 | 2 | 
| Tiếp theo | [1, 1, 1] | [1, 1, 1] | 1 | 3 | 

Điều này cho thấy rằng trong các cấu trúc xen kẽ hoàn toàn, Alice vẫn thu thập một nửa tổng số có sẵn`1`-khối cân, phù hợp với sự xen kẽ tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Nén là tuyến tính, sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Cửa hàng chạy theo chiều dài của từng đoạn | 

Độ dài chuỗi tối đa là 100 nên ngay cả bước sắp xếp cũng không đáng kể. Với tối đa 500 trường hợp thử nghiệm, điều này vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        s = input().strip()
        ones = []
        i = 0
        n = len(s)
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            if s[i] == '1':
                ones.append(j - i)
            i = j
        ones.sort(reverse=True)
        ans = sum(ones[k] for k in range(0, len(ones), 2))
        out.append(str(ans))
    return "\n".join(out)

assert run("5\n01111001\n0000\n111111\n101010101\n011011110111\n") == "4\n0\n6\n3\n6"
assert run("1\n0\n") == "0"
assert run("1\n1\n") == "1"
assert run("1\n101\n") == "1"
assert run("1\n110011\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| không có ai | 
|`1`|`1`| khối đơn | 
|`101`|`1`| những cái riêng biệt | 
|`110011`|`2`| nhiều khối có số 0 | 

## Vỏ cạnh 

Một chuỗi hoàn toàn bằng không như`0000`không tạo ra`1`các phân đoạn, do đó thuật toán trả về 0 một cách chính xác vì danh sách các khối được ghi trống và Alice không bao giờ đạt được điểm. 

Một chuỗi đầy đủ như`111111`nén lại thành một đoạn có chiều dài sáu. Alice thực hiện ngay nước đi đầu tiên và vì không có phân đoạn nào khác nên cô ấy nhận được toàn bộ giá trị mà không bị can thiệp. 

Các cấu trúc xen kẽ như`1010101`sản xuất nhiều phân đoạn đơn vị. Việc sắp xếp duy trì tính độc lập của chúng và việc lựa chọn xen kẽ đảm bảo Alice nắm bắt được mọi phân đoạn khác một cách nhất quán mà không cần mô phỏng những thay đổi về cấu trúc trong chuỗi.
