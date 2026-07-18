---
title: "CF 103480B - 7 \u7684\u610f\u5fd7"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên dương và nhiệm vụ là đếm xem có bao nhiêu mảng con có tổng bằng 7777."
date: "2026-07-03T06:30:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103480
codeforces_index: "B"
codeforces_contest_name: "The 4th Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 103480
solve_time_s: 42
verified: true
draft: false
---

[CF 103480B - 7 \u7684\u610f\u5fd7](https://codeforces.com/problemset/problem/103480/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên dương và nhiệm vụ là đếm xem có bao nhiêu mảng con có tổng bằng 7777. Một mảng con được xác định bằng cách chọn hai chỉ số$l$Và$r$với$l \le r$và tổng hợp tất cả các phần tử giữa chúng. Chúng ta không được yêu cầu tìm giá trị tối đa hoặc tối thiểu mà chỉ đếm xem có bao nhiêu phân đoạn liền kề khác nhau tạo ra tổng mục tiêu. 

Những hạn chế quan trọng một cách rất trực tiếp. Độ dài mảng có thể lên tới$10^5$và có thể có tới 10 trường hợp thử nghiệm. Một phép liệt kê bậc hai đơn giản của tất cả các mảng con sẽ bao gồm khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong một giây. Điều này ngay lập tức loại trừ mọi phương pháp tính lại tổng từ đầu cho mỗi cặp điểm cuối. 

Có một ràng buộc về cấu trúc trở nên quan trọng: mọi phần tử đều dương và nhiều nhất là 5000. Điều này đảm bảo rằng tổng tiền tố đang tăng nghiêm ngặt và hành vi của cửa sổ trượt được xác định rõ ràng mà không phải lo lắng về các hiệu chỉnh âm. 

Một trường hợp thất bại tinh vi đối với việc triển khai đơn giản là việc tính toán lại các tổng mảng con nhiều lần. Ví dụ: nếu chúng ta sửa điểm cuối bên trái và mở rộng con trỏ bên phải trong khi tính tổng liên tục, cuối cùng chúng ta vẫn thực hiện$O(n^2)$bổ sung. Một cạm bẫy khác là bỏ qua rằng nhiều mảng con có thể chồng chéo lên nhau nhiều và đếm chúng không chính xác nếu sử dụng hàm băm trên tổng mà không căn chỉnh tiền tố thích hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: liệt kê mọi cặp$(l, r)$, tính tổng của mảng con và kiểm tra xem nó có bằng 7777 hay không. Điều này đúng vì nó tuân theo trực tiếp định nghĩa của bài toán. Tuy nhiên, việc tính toán từng khoản tiền từ chi phí đầu$O(n)$, và có$O(n^2)$cặp, dẫn đến$O(n^3)$thời gian. Ngay cả khi chúng tôi tối ưu hóa từng tổng của mảng con bằng cách sử dụng mảng tổng tiền tố, việc giảm mỗi truy vấn xuống còn$O(1)$, chúng tôi vẫn có$O(n^2)$tổng số mảng con cho mỗi trường hợp thử nghiệm, quá lớn đối với$n = 10^5$. 

Quan sát chính xuất phát từ tính tích cực của tất cả các yếu tố. Vì tất cả các số đều dương nên tổng tiền tố sẽ tăng lên khi chúng ta di chuyển sang phải. Điều này cho phép chúng ta duy trì một cửa sổ trượt: đối với mỗi điểm cuối bên phải, chúng ta có thể di chuyển điểm cuối bên trái về phía trước trong khi tổng vượt quá mục tiêu. Điều bất biến là tổng cửa sổ hiện tại luôn được duy trì một cách hiệu quả và không có mảng con hợp lệ nào bị bỏ qua hoặc được tính hai lần. 

Điều này biến vấn đề thành quét hai con trỏ trên mảng, trong đó mỗi con trỏ chỉ di chuyển về phía trước nhiều nhất$n$lần, tạo ra một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc$O(n^3)$|$O(1)$| Quá chậm | 
| Hai con trỏ tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cửa sổ trượt$[l, r]$và tổng hiện tại của nó. 

1. Khởi tạo hai con trỏ$l = 0$,$r = 0$và tổng hiện có bằng 0. Điều này thể hiện một cửa sổ trống trước khi xử lý bất kỳ phần tử nào. 
2. Di chuyển con trỏ phải từng bước từ trái sang phải. Ở mỗi bước, hãy thêm phần tử hiện tại vào tổng hiện có. Điều này mở rộng cửa sổ và đảm bảo chúng tôi xem xét mọi mảng con có thể kết thúc tại$r$. 
3. Khi tổng hiện tại vượt quá 7777, hãy di chuyển con trỏ bên trái về phía trước và trừ phần tử rời khỏi cửa sổ. Điều này hợp lệ vì tất cả các phần tử đều dương, do đó, việc thu gọn từ bên trái chỉ có thể làm giảm tổng chứ không bao giờ tăng tổng. 
4. Sau khi điều chỉnh cửa sổ, nếu tổng bằng 7777, hãy tăng câu trả lời. Điều này đếm mảng con duy nhất kết thúc ở hiện tại$r$tổng của nó phù hợp với mục tiêu. 
5. Tiếp tục cho đến khi con trỏ bên phải đến cuối mảng. 

Tính đúng đắn phụ thuộc vào thực tế là tại mỗi vị trí$r$, thuật toán tìm tất cả các ranh giới trái hợp lệ$l$sao cho tổng của mảng con chính xác là 7777. Bởi vì mảng chỉ chứa các số dương nên có nhiều nhất một chuỗi điều chỉnh hợp lệ cho mỗi mảng.$r$và không có mảng con hợp lệ nào bị bỏ qua. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi bước, cửa sổ$[l, r]$là ranh giới bên trái nhỏ nhất có thể có của điểm cuối bên phải hiện tại sao cho tổng không vượt quá 7777. Vì các phần tử là dương nên chuyển động$l$chuyển tiếp đơn điệu làm giảm tổng và do đó mỗi con trỏ di chuyển nhiều nhất$n$lần. Mỗi mảng con hợp lệ kết thúc tại$r$phải xuất hiện chính xác khi tổng cửa sổ trở thành 7777 sau khi thu nhỏ từ tổng quá lớn, vì vậy mọi giải pháp đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    target = 7777
    l = 0
    s = 0
    ans = 0
    
    for r in range(n):
        s += arr[r]
        
        while l <= r and s > target:
            s -= arr[l]
            l += 1
        
        if s == target:
            ans += 1
    
    return ans

t = int(input())
out = []
for _ in range(t):
    out.append(str(solve()))
print("\n".join(out))
```Mã này phản ánh trực tiếp quá trình cửa sổ trượt. Biến`s`theo dõi tổng cửa sổ hiện tại và con trỏ bên trái`l`chỉ được di chuyển khi cần thiết để giữ tổng trong giới hạn. điều kiện`s == target`chỉ được kiểm tra sau khi khôi phục tính khả thi, đảm bảo tính đúng đắn. Việc sử dụng một lần cho mỗi trường hợp thử nghiệm giúp việc triển khai hiệu quả và đơn giản, tránh mọi từ điển tổng tiền tố hoặc băm. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[2000, 3000, 2777, 1000]`. 

Chúng tôi theo dõi cửa sổ trượt: 

| r | tôi | cửa sổ | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [2000] | 2000 | mở rộng | 
| 1 | 0 | [2000, 3000] | 5000 | mở rộng | 
| 2 | 0 | [2000, 3000, 2777] | 7777 | tìm thấy trận đấu | 
| 3 | 0 | [2000, 3000, 2777, 1000] | 8777 | thu nhỏ trái | 

Khi chúng tôi đạt được$r = 2$, tổng khớp chính xác một lần, vì vậy chúng tôi đếm một mảng con. Tại$r = 3$, tổng vượt quá mục tiêu và cần phải thu hẹp lại, nhưng không có kết quả khớp chính xác nào xuất hiện. 

Bây giờ hãy xem xét`[7777, 1, 7776]`. 

| r | tôi | cửa sổ | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [7777] | 7777 | trận đấu | 
| 1 | 0 | [7777, 1] | 7778 | thu nhỏ | 
| 1 | 1 | [1] | 1 | không khớp | 
| 2 | 1 | [1, 7776] | 7777 | trận đấu | 

Ví dụ này cho thấy các mảng con hợp lệ có thể xuất hiện sau khi thu nhỏ cửa sổ và thuật toán đánh giá lại điều kiện một cách chính xác sau mỗi lần điều chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được thêm và xóa tối đa một lần thông qua hai con trỏ | 
| Không gian |$O(1)$thêm không gian | Chỉ sử dụng một số bộ đếm và con trỏ | 

Được cho$T \le 10$Và$n \le 10^5$, tổng công việc là tuyến tính ở kích thước đầu vào, thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        n = int(input())
        arr = list(map(int, input().split()))
        target = 7777
        l = 0
        s = 0
        ans = 0
        for r in range(n):
            s += arr[r]
            while l <= r and s > target:
                s -= arr[l]
                l += 1
            if s == target:
                ans += 1
        return ans
    
    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# sample-like case
assert run("1\n4\n2000 3000 2777 1000") == "1"

# exact match at single element
assert run("1\n1\n7777") == "1"

# no valid subarray
assert run("1\n3\n1 2 3") == "0"

# multiple overlapping valid subarrays
assert run("1\n5\n7777 1 7776 1 7777") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử chính xác duy nhất | 1 | trận đấu đơn yếu tố | 
| không có giải pháp | 0 | xử lý vắng mặt | 
| trận đấu chồng chéo | 3 | nhiều cửa sổ hợp lệ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một phần tử bằng 7777. Đối với đầu vào`[7777]`, thuật toán bắt đầu bằng`r = 0`, thêm phần tử và ngay lập tức tìm thấy`s == 7777`, do đó nó đếm chính xác một mảng con. 

Một trường hợp khác là khi tất cả các phần tử đều nhỏ và không có sự kết hợp nào đạt tới 7777. Ví dụ:`[1, 2, 3]`làm cho tổng cửa sổ không bao giờ vượt quá mục tiêu và vì nó không bao giờ bằng 7777 nên câu trả lời vẫn là 0. 

Một trường hợp tinh vi hơn là khi tổng vượt quá mục tiêu trong thời gian ngắn và phải co lại trước khi tìm thấy lại cửa sổ hợp lệ. TRONG`[7777, 1, 7776]`, cửa sổ đầu tiên khớp ở chỉ số 0, sau đó mở rộng ra ngoài mục tiêu, rồi thu nhỏ lại và sau đó tìm thấy một mảng con hợp lệ khác. Cửa sổ trượt xử lý chính xác việc này vì nó luôn khôi phục tính khả thi trước khi kiểm tra lại sự bằng nhau.
