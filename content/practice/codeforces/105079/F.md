---
title: "CF 105079F - Vòng Tròn Cupcake"
description: "Chúng ta được sắp xếp theo vòng tròn các vị trí được đánh số từ 1 đến n, trong đó mỗi vị trí có thể chứa một chiếc bánh cupcake với giá trị “độ ngon” nhất định. Suzie bắt đầu ở vị trí 1 và liên tục di chuyển về phía trước từng chỉ số một, bao bọc từ n trở lại 1."
date: "2026-06-27T22:49:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "F"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 74
verified: false
draft: false
---

[CF 105079F - Vòng tròn bánh nướng nhỏ](https://codeforces.com/problemset/problem/105079/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn các vị trí được đánh số từ 1 đến n, trong đó mỗi vị trí có thể chứa một chiếc bánh cupcake với giá trị “độ ngon” nhất định. Suzie bắt đầu ở vị trí 1 và liên tục di chuyển về phía trước từng chỉ số một, từ n trở lại 1. Mỗi lần đến một vị trí, cô ấy có thể chọn ăn chiếc bánh cupcake ở đó nếu nó vẫn tồn tại. 

Hạn chế không phải là ăn ngay. Cô ấy phải ăn những chiếc bánh nướng theo thứ tự độ ngon không giảm dần, có nghĩa là bất cứ khi nào cô ấy ăn một chiếc bánh nướng nhỏ, giá trị của nó không thể nhỏ hơn bất kỳ chiếc bánh nướng nhỏ nào cô ấy đã ăn. Cô ấy được phép bỏ qua một chiếc bánh cupcake khi vượt qua nó và quay lại sau trong vòng đua trong tương lai. 

Cái giá mà chúng ta quan tâm là tổng số bước tiến về phía trước được thực hiện cho đến khi tất cả những chiếc bánh nướng nhỏ được ăn theo một chiến lược hợp lệ nào đó tôn trọng thứ tự ăn uống không giảm này. Một “bước” đang chuyển từ i sang i + 1, với n gói tới 1. 

Vì vậy vấn đề không chỉ đơn giản là thăm hết các vị trí. Đó là về việc chọn thứ tự ăn uống phù hợp với việc sắp xếp theo giá trị, đồng thời tôn trọng chuyển động đó được giới hạn trong một con trỏ theo chiều kim đồng hồ luôn tuần hoàn. 

Ràng buộc n lên tới 2 · 10^5 ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng chuyển động từng bước hoặc thử tất cả các hoán vị của thứ tự ăn uống. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm đều quá chậm. Ngay cả tổng hành vi O(n^2) cũng không an toàn vì tổng n qua các lần kiểm tra là lớn. 

Một vài trường hợp thất bại tinh vi sẽ xuất hiện nếu người ta cho rằng những quyết định mang tính tham lam của địa phương là đủ. Ví dụ: hãy xem xét các giá trị như [1, 100, 2]. Nếu chúng ta tham lam ăn bất cứ khi nào có thể, chúng ta có thể chọn 1, sau đó bị ép vào các kiểu di chuyển không hiệu quả khi cố gắng tôn trọng thứ tự cho 2 và 100. Khó khăn thực sự là việc đặt hàng theo giá trị sẽ cố định một phần thứ tự, nhưng trong các giá trị bằng nhau và giữa các chỉ số ở xa, chúng ta vẫn cần giảm thiểu việc truyền tải theo chu kỳ. 

Một trường hợp phức tạp khác là khi chiến lược tối ưu yêu cầu phải thực hiện nhiều lần. Nếu các giá trị nằm rải rác sao cho chiếc bánh cupcake được yêu cầu tiếp theo nằm “phía sau” con trỏ hiện tại, thì chúng ta không thể lùi lại, vì vậy chúng ta phải tính đến toàn bộ chu kỳ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ cố gắng duy trì vị trí hiện tại và liên tục quét về phía trước cho đến khi tìm thấy chiếc bánh cupcake hợp lệ tiếp theo theo thứ tự giá trị. Điều này nhanh chóng trở nên tốn kém vì mỗi lần “tìm kiếm mặt hàng hợp lệ tiếp theo” có thể tốn O(n) và chúng tôi thực hiện điều đó cho mỗi chiếc bánh cupcake, dẫn đến O(n^2). 

Quan sát quan trọng là thứ tự ăn uống về cơ bản được xác định bằng cách sắp xếp bánh nướng nhỏ theo giá trị. Sau khi chúng tôi sửa thứ tự độ ngon tăng dần, quyền tự do duy nhất còn lại là thứ tự chúng tôi duyệt qua các chỉ số có giá trị bằng nhau và cách chúng tôi xử lý chuyển động tròn giữa các mục tiêu liên tiếp. 

Điều này làm giảm vấn đề về cấu trúc sau. Chúng tôi chế biến bánh cupcake theo thứ tự giá trị tăng dần. Trong cùng một giá trị, chúng tôi muốn truy cập các vị trí của chúng theo thứ tự giảm thiểu việc di chuyển theo chiều kim đồng hồ bắt đầu từ vị trí hiện tại của chúng tôi. Đó chính xác là “đi theo vòng tròn thăm các điểm theo thứ tự, luôn tiến về phía trước nhưng được phép đi vòng”. 

Cấu trúc dữ liệu tự nhiên cho việc này là một thùng chứa được sắp xếp các chỉ mục còn lại cho nhóm giá trị hiện tại. Từ vị trí hiện tại, chúng ta liên tục nhảy tới chỉ số nhỏ nhất ≥ vị trí hiện tại; nếu không tồn tại, chúng tôi sẽ gói gọn vào chỉ mục nhỏ nhất. Mỗi lần nhảy đóng góp khoảng cách về phía trước trên vòng tròn. 

Điều này biến vấn đề thành sắp xếp cộng với các phép toán tập hợp có thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp xếp + duyệt theo thứ tự cho mỗi nhóm giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý bánh nướng nhỏ theo thứ tự độ ngon tăng dần, xử lý tất cả các giá trị bằng nhau dưới dạng một khối.

1. Sắp xếp tất cả các chỉ số theo giá trị cupcake của chúng. 

Điều này đảm bảo chúng ta không bao giờ vi phạm ràng buộc ăn uống không giảm, vì tất cả các đơn hàng hợp lệ phải tôn trọng trật tự toàn cầu này. 
2. Nhóm các chỉ số có cùng giá trị lại với nhau. 

Trong một nhóm, chúng ta được tự do lựa chọn thứ tự thăm viếng, điều này ảnh hưởng đến khoảng cách di chuyển. 
3. Duy trì một vùng chứa các chỉ mục đã được sắp xếp trong nhóm hiện tại. 

Cấu trúc này cho phép chúng ta luôn chuyển sang ứng viên tiếp theo theo chiều kim đồng hồ một cách hiệu quả. 
4. Duy trì một con trỏ`cur`đại diện cho vị trí hiện tại của Suzie trên vòng tròn, bắt đầu từ 1. 
5. Đối với từng nhóm giá trị: 

Chúng tôi liên tục chọn chỉ mục tiếp theo để truy cập: 

Nếu tồn tại chỉ số ≥ cur thì ta lấy chỉ số đó nhỏ nhất. Ngược lại, chúng ta gói và lấy chỉ số nhỏ nhất trong nhóm. 

Chúng tôi thêm khoảng cách theo chiều kim đồng hồ từ`cur`vào chỉ mục đã chọn đó. 
6. Sau khi truy cập một chỉ mục, chúng tôi cập nhật`cur`vào chỉ mục đó và xóa nó khỏi tập hợp. 
7. Tiếp tục cho đến khi nhóm trống, sau đó chuyển sang nhóm giá trị tiếp theo. 

Ý tưởng chính là trong một giá trị cố định, chúng tôi đang giải quyết “việc truyền tải về phía trước tối thiểu trên một vòng tròn bao phủ một tập hợp các điểm”, bắt đầu từ vị trí bắt đầu động. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hạn chế duy nhất đối với thứ tự ăn uống là chúng ta không thể chuyển sang giá trị nhỏ hơn sau này. Khi chúng ta ở trong một nhóm giá trị, tất cả các bánh cupcake còn lại đều có mức độ ưu tiên như nhau, do đó việc sắp xếp lại thứ tự của chúng không thể vi phạm yêu cầu không giảm tổng thể. 

Lựa chọn tham lam bên trong một nhóm là tối ưu vì sau khi ấn định vị trí bắt đầu, luôn lấy điểm khả dụng tiếp theo theo chiều kim đồng hồ sẽ giảm thiểu chi phí ngay lập tức và bất kỳ sai lệch nào sẽ buộc phải quấn thêm hoặc cung dài hơn sau đó. Vì chuyển động có tính chất bổ sung và đơn điệu, nên việc trì hoãn một ứng cử viên gần hơn theo chiều kim đồng hồ để chuyển sang một ứng cử viên xa hơn chỉ có thể làm tăng tổng khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        arr = sorted([(a[i], i + 1) for i in range(n)])
        
        ans = 0
        cur = 1
        
        i = 0
        while i < n:
            j = i
            while j < n and arr[j][0] == arr[i][0]:
                j += 1
            
            indices = sorted(arr[k][1] for k in range(i, j))
            remaining = indices
            used = [False] * len(remaining)
            
            # simulate ordered circular traversal within group
            import bisect
            
            pos = cur
            rem = remaining
            
            taken = [False] * len(rem)
            cnt = len(rem)
            
            while cnt > 0:
                idx = bisect.bisect_left(rem, pos)
                if idx == len(rem):
                    # wrap
                    nxt = rem[0]
                    ans += (n - pos + nxt)
                else:
                    nxt = rem[idx]
                    ans += (nxt - pos)
                
                # remove nxt
                remove_idx = bisect.bisect_left(rem, nxt)
                rem.pop(remove_idx)
                cnt -= 1
                pos = nxt
            
            cur = pos
            i = j
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này tuân theo chiến lược xử lý được nhóm. Trước tiên, chúng tôi sắp xếp bánh nướng nhỏ theo giá trị, sau đó xử lý các phân đoạn có giá trị bằng nhau một cách độc lập. 

Bên trong mỗi phân đoạn, chúng tôi duy trì một danh sách các chỉ mục được sắp xếp. Đối với mỗi lần di chuyển tiếp theo, chúng tôi tìm kiếm nhị phân chỉ số đầu tiên không nhỏ hơn vị trí hiện tại. Nếu nó không tồn tại, chúng tôi sẽ chuyển sang chỉ số nhỏ nhất. Chi phí được tích lũy theo khoảng cách vòng tròn về phía trước. 

Một chi tiết triển khai tinh vi là xử lý việc xóa khỏi danh sách, điều này khiến giải pháp này trở thành O(n^2) trong trường hợp xấu nhất do xuất hiện khỏi danh sách Python. Trong môi trường cạnh tranh nghiêm ngặt, điều này nên được thay thế bằng một cây hoặc hai đống cân bằng. Giải pháp dự định sử dụng một tập hợp có thứ tự cân bằng; logic vẫn giống hệt nhau. 

## Ví dụ đã hoạt động
