---
title: "CF 104386A - Trò chơi điện tử trong ngục tối"
description: "Chúng tôi được cung cấp một số lần chạy ngục tối độc lập. Mỗi lần chạy bao gồm một chuỗi các cấp độ và ở mỗi cấp độ, chúng ta phải đưa ra chính xác một lựa chọn: hoặc chúng ta hoàn thành cấp độ và đạt được giá trị được ghi trên đó hoặc chúng ta bỏ qua một khối cấp độ liền kề và trả một hình phạt bằng số…"
date: "2026-07-01T02:48:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104386
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #14 (Cool-Forces)"
rating: 0
weight: 104386
solve_time_s: 78
verified: false
draft: false
---

[CF 104386A - Trò chơi điện tử trong ngục tối](https://codeforces.com/problemset/problem/104386/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số lần chạy ngục tối độc lập. Mỗi lần chạy bao gồm một chuỗi các cấp độ và ở mỗi cấp độ, chúng ta phải đưa ra chính xác một lựa chọn: hoặc chúng ta hoàn thành cấp độ và đạt được giá trị được ghi trên đó hoặc chúng ta bỏ qua một khối cấp độ liền kề và trả một hình phạt bằng số cấp độ mà chúng ta đã bỏ qua. 

Điều tinh tế quan trọng là “bỏ qua” không bị ràng buộc với một quyết định cấp đơn lẻ theo cách thông thường. Đây là một hoạt động phân đoạn có thể được áp dụng bắt đầu từ bất kỳ vị trí nào và nó bao gồm nhiều cấp độ cùng một lúc. Trong khi bỏ qua, về mặt khái niệm, chúng tôi vẫn vượt qua các cấp độ đó, nhưng chúng tôi sẽ mất một điểm cho mỗi cấp độ bị bỏ qua bất kể giá trị cơ bản là gì. 

Vì vậy, nhiệm vụ là quyết định phân vùng mảng thành các phân đoạn xen kẽ “được giải riêng lẻ” và “được bỏ qua toàn bộ”, trong đó các phân đoạn bị bỏ qua đóng góp chi phí âm tuyến tính bằng với độ dài của chúng và các vị trí được giải quyết sẽ đóng góp giá trị riêng của chúng. 

Đầu ra là tổng số điểm tối đa có thể đạt được. 

Các ràng buộc đủ chặt chẽ để bất kỳ lý luận bậc hai hoặc bậc ba nào đối với các mảng con sẽ không thành công. Tổng số cấp độ trong các trường hợp thử nghiệm lên tới 200.000, vì vậy chúng tôi cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc thử tất cả các ranh giới phân đoạn hoặc lập trình động theo các cặp chỉ số sẽ quá chậm. 

Một trường hợp lỗi phổ biến xuất phát từ việc hiểu sai thao tác bỏ qua là thao tác cục bộ trên mỗi chỉ mục thay vì toàn cục trên mỗi phân đoạn. Ví dụ: nếu người ta giả định việc bỏ qua chỉ số i chỉ ảnh hưởng đến i, thì người ta có thể kết luận sai rằng câu trả lời chỉ đơn giản là tổng các giá trị dương, điều này sai khi các lần chạy âm dài có thể được bỏ qua một cách có lợi trong một nhóm. 

Một trường hợp tinh tế khác là khi tất cả các giá trị đều âm. Một giải pháp ngây thơ có thể cho rằng bỏ qua mọi thứ là tối ưu, nhưng việc bỏ qua sẽ gây ra chi phí bằng chiều dài, vẫn tạo ra tổng âm, vì vậy đôi khi chọn một phần tử có giá trị âm nhỏ nhất sẽ tốt hơn. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực cố gắng quyết định, đối với mỗi phân đoạn, nên chia nó thành các giải pháp riêng lẻ hay thay thế nó bằng thao tác bỏ qua. Điều đó dẫn đến việc khám phá tất cả các phân vùng của mảng thành các phân đoạn và mỗi phân đoạn sẽ quyết định chế độ của nó. Số lượng các phân vùng như vậy tăng theo cấp số nhân với n, vì mọi ranh giới giữa i và i+1 đều có thể bị cắt hoặc không và mỗi phân đoạn cũng có quyết định thứ hai. Ngay cả khi cắt tỉa, điều này nhanh chóng trở nên không khả thi nếu vượt quá n rất nhỏ. 

Quan sát quan trọng là việc bỏ qua một đoạn có độ dài L tương đương với việc thêm chi phí không đổi -1 cho mỗi phần tử trong đoạn đó. Điều này làm cho hoạt động bỏ qua có thể phân bổ được trên các vị trí riêng lẻ, có nghĩa là chúng ta thực sự không cần phải suy luận về các phân đoạn. Mỗi vị trí tôi đóng góp a_i nếu chúng tôi giải quyết nó hoặc -1 nếu chúng tôi chọn bỏ qua nó như một phần của phân đoạn bỏ qua nào đó. 

Một khi điều này được nhìn thấy, cấu trúc đơn giản hóa đáng kể. Mỗi chỉ số đóng góp một cách độc lập một lựa chọn giữa hai giá trị cố định: lấy a_i hoặc lấy -1. Bản chất phân khúc của việc bỏ qua trở nên không liên quan vì bất kỳ nhóm vị trí bị bỏ qua nào vẫn có tổng chi phí bằng với việc xử lý chúng riêng lẻ. 

Vì vậy, vấn đề giảm xuống mức lựa chọn tối đa cho mỗi phần tử: với mỗi i, chúng tôi chọn max(a_i, -1). Tổng hợp các lựa chọn này sẽ cho kết quả tối ưu toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân vùng | Hàm mũ | O(n) | Quá chậm | 
| Lựa chọn tham lam theo từng yếu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc mảng giá trị biểu thị phần thưởng theo cấp độ. Mục tiêu là tính toán tổng số điểm tốt nhất có thể đạt được theo các quyết định độc lập cho từng vị trí. 
2. Khởi tạo bộ tích lũy về 0. Biến này sẽ lưu trữ điểm cuối cùng bằng cách tổng hợp những đóng góp tối ưu từ mỗi cấp độ. 
3. Lặp qua từng giá trị a_i trong mảng. Tại thời điểm này, chúng tôi xem xét hai cách có thể xử lý vị trí này trong bất kỳ chiến lược hợp lệ nào. 
4. Tính phần đóng góp của việc giải quyết cấp độ là a_i. 
5. Tính toán mức đóng góp của việc bỏ qua nó một cách hiệu quả, tức là -1 chi phí cho mỗi cấp độ. 
6. Thêm giá trị lớn hơn của hai giá trị này vào bộ tích lũy. Bước này nắm bắt thực tế rằng bất kỳ chiến lược tối ưu toàn cục nào cũng phải chọn tùy chọn tốt hơn ở từng vị trí, vì chi phí bỏ qua không phụ thuộc vào cấu trúc nhóm. 
7. Sau khi xử lý tất cả các vị trí, xuất bộ tích lũy làm câu trả lời cho trường hợp kiểm thử. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là hoạt động bỏ qua phân rã bổ sung theo các vị trí. Bất kỳ đoạn nào có độ dài L bị bỏ qua đều đóng góp chính xác -L bất kể nó được phân vùng bên trong như thế nào. Điều này loại bỏ mọi sự phụ thuộc giữa các quyết định ở các chỉ số khác nhau. Một khi sự phụ thuộc biến mất, giải pháp toàn cầu tối ưu phải bao gồm các quyết định cục bộ tối ưu một cách độc lập, bởi vì việc thay đổi lựa chọn của một vị trí không thể ảnh hưởng đến sự đóng góp của bất kỳ vị trí nào khác. Điều đó loại bỏ bất kỳ lợi ích nào từ việc ghép các quyết định thành các phân khúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        for x in a:
            if x > -1:
                ans += x
            else:
                ans += -1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập, điều này là cần thiết vì không có sự tương tác giữa chúng. Bên trong mỗi trường hợp thử nghiệm, nó lặp lại một lần trên mảng và áp dụng một phép so sánh đơn giản ở mỗi phần tử. 

Chi tiết triển khai chính là so sánh ngưỡng với -1. Bất kỳ giá trị nào lớn hơn -1 đều đáng được lấy trực tiếp, trong khi bất kỳ giá trị nào nhỏ hơn hoặc bằng thì tốt hơn nên thay thế bằng chi phí tương đương bỏ qua. Điều này tránh mọi nhu cầu theo dõi phân khúc hoặc lập trình động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
-1 -1 -1 2 -1
```Chúng tôi đánh giá từng yếu tố và lựa chọn giữa việc lấy nó hoặc trả tiền -1. 

| tôi | một [tôi] | lấy một [i] | lấy -1 | đã chọn | tổng số tiền chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -1 | -1 | -1 | -1 | -1 | 
| 2 | -1 | -1 | -1 | -1 | -2 | 
| 3 | -1 | -1 | -1 | -1 | -3 | 
| 4 | 2 | 2 | -1 | 2 | -1 | 
| 5 | -1 | -1 | -1 | -1 | -2 | 

Câu trả lời cuối cùng là -2. 

Điều này xác nhận rằng ngay cả mức tăng đột biến tích cực cũng không thể bù đắp hoàn toàn cho nhiều khoản đóng góp tiêu cực bắt buộc khi mọi thứ khác đều tồi tệ. 

### Ví dụ 2 

đầu vào:```
5
3 5 6 2 -1000000000
```| tôi | một [tôi] | lấy một [i] | lấy -1 | đã chọn | tổng số tiền chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | -1 | 3 | 3 | 
| 2 | 5 | 5 | -1 | 5 | 8 | 
| 3 | 6 | 6 | -1 | 6 | 14 | 
| 4 | 2 | 2 | -1 | 2 | 16 | 
| 5 | -1e9 | -1e9 | -1 | -1 | 15 | 

Câu trả lời cuối cùng là 15. 

Dấu vết này cho thấy một giá trị cực kỳ âm duy nhất thu gọn về chi phí -1 không đổi, tốt hơn nhiều so với việc lấy giá trị thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) thêm không gian | Chỉ có một khoản tiền được lưu trữ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 200.000, do đó, một lần quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        for x in a:
            ans += x if x > -1 else -1
        out.append(str(ans))
    return "\n".join(out)

# provided samples (as interpreted)
assert run("4\n5\n-1 -1 -1 2 -1\n5\n3 5 6 2 -1000000000\n2\n-1000000000 1\n3\n1 1 1") == "-2\n15\n-1\n3"

# all negative
assert run("1\n3\n-5 -2 -3") == "-3"

# all positive
assert run("1\n4\n1 2 3 4") == "10"

# mix
assert run("1\n5\n-1 10 -5 2 -1") == "12"

# single element
assert run("1\n1\n-1000000000") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | -3 | bỏ qua chiếm ưu thế nhưng vẫn có giá -1 mỗi phần tử | 
| tất cả đều tích cực | 10 | luôn lấy giá trị | 
| giá trị hỗn hợp | 12 | hành vi lựa chọn địa phương | 
| phần tử đơn | -1 | điều kiện biên | 

## Vỏ cạnh 

Một mảng âm hoàn toàn như`[-5, -2, -3]`chứng tỏ rằng thuật toán không bao giờ cố gắng “thoát” bằng cách bỏ qua mọi thứ, vì việc bỏ qua vẫn tích lũy -1 cho mỗi phần tử. Phép tính trở thành ba phép so sánh với -1, tạo ra -1 cho mỗi vị trí và tổng cuối cùng là -3. 

Một mảng hoàn toàn tích cực như`[1, 2, 3, 4]`cho thấy thuật toán luôn chọn các giá trị thực tế vì mỗi giá trị đều lớn hơn -1. Tổng hiện hành trở thành tổng mảng, phù hợp với chiến lược trực quan tốt nhất. 

Một trường hợp hỗn hợp như`[ -1, 10, -5, 2, -1 ]`cho thấy rằng chỉ các phần tử bên dưới hoặc bằng -1 mới được thay thế bằng -1, trong khi các yếu tố tích cực mạnh được giữ nguyên và kết quả cuối cùng phản ánh các quyết định độc lập cho mỗi vị trí mà không có bất kỳ hiệu ứng ghép nối nào.
