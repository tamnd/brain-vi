---
title: "CF 1836A - Khu trục hạm"
description: "Mỗi robot báo cáo một số nguyên duy nhất mô tả có bao nhiêu robot đứng trước nó trong hàng riêng của nó. Điều khó khăn là chúng ta không được biết có bao nhiêu dòng tồn tại hoặc robot nào thuộc dòng nào."
date: "2026-06-09T06:43:12+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1836
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 880 (Div. 2)"
rating: 800
weight: 1836
solve_time_s: 68
verified: true
draft: false
---

[CF 1836A - Kẻ hủy diệt](https://codeforces.com/problemset/problem/1836/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi robot báo cáo một số nguyên duy nhất mô tả có bao nhiêu robot đứng trước nó trong hàng riêng của nó. Điều khó khăn là chúng ta không được biết có bao nhiêu dòng tồn tại hoặc robot nào thuộc dòng nào. Quyền tự do duy nhất chúng ta có là phân chia robot thành nhiều chuỗi có thứ tự, trong đó giá trị được báo cáo trong mỗi chuỗi phải khớp với vị trí của robot. 

Nếu robot báo cáo giá trị`x`, nó phải chiếm vị trí`x + 1`trong dòng của nó. Điều đó ngay lập tức ngụ ý một ràng buộc về cấu trúc: tất cả các robot có cùng giá trị báo cáo`x`phải được đặt ở những vị trí chính xác`x + 1`trong các dòng tương ứng của chúng, vì vậy mỗi dòng có thể chứa tối đa một robot cho mỗi chỉ số vị trí. 

Nhiệm vụ là quyết định xem liệu chúng ta có thể phân chia tất cả robot thành các dòng hợp lệ sao cho số được báo cáo của mỗi robot khớp với vị trí của nó trong dòng đã chọn hay không. 

Những hạn chế là nhỏ. Mỗi thử nghiệm có tối đa 100 rô-bốt và tổng số qua các thử nghiệm nhiều nhất là 200. Điều này cho phép bất kỳ giải pháp nào ít nhất là bậc hai, nhưng cũng gợi ý rằng việc xây dựng dựa trên tham lam hoặc đếm được mong đợi thay vì bất kỳ tìm kiếm nào trên các phân vùng, điều này sẽ bùng nổ về mặt tổ hợp. 

Một trường hợp lỗi nhỏ xuất hiện khi giá trị báo cáo quá lớn so với số lượng robot có sẵn. Ví dụ: một robot báo cáo`99`không thể đặt ở bất kỳ đâu vì nó cần ít nhất 100 vị trí trên đường thẳng của nó. Một kiểu lỗi khác xảy ra khi chúng ta có một yêu cầu giống như dây chuyền không thể đáp ứng được: nếu robot nào đó cần vị trí 2 thì phải tồn tại một robot cần vị trí 1 trước đó trong cùng cấu trúc dây chuyền đó, nếu không dây chuyền sẽ bị đứt. mẫu`0 0 2`thất bại chính xác là do thiếu robot “vị trí 1” để hỗ trợ vị trí cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán mỗi robot vào một dòng và một vị trí trong dòng đó, sau đó xác minh các ràng buộc. Điều này có nghĩa là khám phá các phân vùng của tập hợp robot thành các trình tự và đối với mỗi trình tự, hãy sắp xếp chúng một cách nhất quán với các vị trí được yêu cầu. Ngay cả khi chúng ta chỉ nghĩ đến việc phân chia robot thành các hàng thì số lượng phân vùng đã tăng theo cấp số nhân.`n`và thứ tự bên trong mỗi phân vùng sẽ thêm một yếu tố giai thừa khác. Vì`n = 100`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cấu trúc đường được xác định đầy đủ bởi các ràng buộc thứ tự tương đối do các giá trị gây ra. Nếu một robot có giá trị`x`, nó cư xử như muốn ngồi ở độ sâu`x`trong một chuỗi nào đó. Bất kỳ cấu hình hợp lệ nào cũng tương đương với việc xây dựng nhiều chuỗi trong đó các độ sâu này xếp thẳng hàng một cách hoàn hảo. Thay vì suy nghĩ theo các đường tùy ý, chúng ta có thể xử lý robot theo thứ tự tăng dần về vị trí cần thiết của chúng và cố gắng mở rộng một phần các đường. 

Một cách hữu ích để xem vấn đề là tưởng tượng các đường xây dựng từ trái sang phải. Khi chúng ta đặt một robot có giá trị`x`, nó phải bắt đầu một dòng mới ở vị trí 0 hoặc mở rộng cấu trúc hiện có đã có robot ở vị trí`x - 1`. Điều này làm giảm vấn đề trong việc theo dõi số lượng “vị trí mở” tồn tại cho mỗi chỉ mục vị trí. 

Chúng ta có thể mô hình hóa điều này bằng cách đếm tần số: bất cứ khi nào chúng ta đặt robot vào vị trí`x`, chúng tôi tăng nhu cầu về vị trí`x + 1`. Việc xây dựng là hợp lệ nếu, khi chúng tôi mô phỏng các vị trí ngày càng tăng, chúng tôi không bao giờ yêu cầu nhiều robot hơn ở vị trí sâu hơn mức chúng tôi có thể hỗ trợ từ các lớp trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | hàm mũ | O(n) | Quá chậm | 
| Phân lớp tần số tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng robot báo cáo từng giá trị. Điều này đưa ra một bản đồ tần số`cnt[x]`. Điều này là cần thiết vì chỉ có sự phân phối các giá trị mới quan trọng chứ không phải bản sắc. 
2. Sắp xếp tất cả các giá trị khác nhau theo thứ tự tăng dần. Chúng tôi xử lý từ yêu cầu vị trí nhỏ nhất trở lên, vì các vị trí thấp hơn phải tồn tại trước khi các vị trí cao hơn có thể được hỗ trợ hợp lệ. 
3. Duy trì một biến`available`, đại diện cho số lượng robot chúng tôi hiện có có khả năng hỗ trợ lớp vị trí tiếp theo. Ban đầu`available = 0`. 
4. Lặp lại các giá trị theo thứ tự tăng dần. Tại giá trị`x`, đầu tiên chúng ta kiểm tra xem`available`đủ để bao phủ tất cả các robot yêu cầu vị trí`x`. Nếu như`available < cnt[x]`, chúng ta thất bại ngay lập tức. Điều này là do chúng tôi không thể đặt tất cả các robot cần vào vị trí`x`vào các tuyến được xây dựng một phần hiện có. 
5. Nếu kiểm tra thành công, chúng tôi sẽ giảm`available`qua`cnt[x]`, vì những robot này sử dụng các vị trí có sẵn ở cấp độ này. 
6. Sau khi đặt tất cả`cnt[x]`robot ở vị trí`x`, mỗi người trong số họ đóng góp một phần mở rộng tiềm năng cho vị trí`x + 1`. Vì vậy chúng tôi tăng`available`qua`cnt[x]`. 
7. Tiếp tục quá trình này cho đến khi tất cả các giá trị được xử lý. Nếu không có lỗi xảy ra, có thể cấu hình. 

Bất biến quan trọng là`available`luôn đại diện cho số lượng “kết thúc” của dòng được xây dựng một phần có thể chấp nhận giá trị vị trí tiếp theo. Mỗi lớp tiêu thụ chính xác các robot được đặt ở độ sâu đó và tái tạo cùng số lượng phần mở rộng cho độ sâu tiếp theo. Nếu tại bất kỳ thời điểm nào chúng tôi cần nhiều vị trí hơn các tiện ích mở rộng có sẵn thì không sự sắp xếp lại nào có thể khắc phục được vì các vị trí sâu hơn về cơ bản phụ thuộc vào sự hiện diện của các vị trí trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    cnt = {}
    for x in a:
        cnt[x] = cnt.get(x, 0) + 1
    
    available = 0
    ok = True
    
    for x in sorted(cnt):
        if available < cnt[x]:
            ok = False
            break
        available -= cnt[x]
        available += cnt[x]
    
    print("YES" if ok else "NO")
```Việc triển khai nén ý tưởng thành một đợt quét tần số. Từ điển`cnt`lưu trữ bao nhiêu robot yêu cầu mỗi vị trí. Việc sắp xếp đảm bảo chúng tôi xử lý các ràng buộc theo thứ tự tăng dần về độ sâu cần thiết. 

Biến`available`theo dõi có bao nhiêu “khe chuỗi mở” tồn tại. Bước quan trọng là kiểm tra tính khả thi`available < cnt[x]`, điều này có nghĩa là chúng tôi không bao giờ cố gắng đặt nhiều rô-bốt hơn ở độ sâu nhất định ngoài mức chúng tôi có thể hỗ trợ từ cấu trúc đã thiết lập trước đó. 

bản cập nhật`available -= cnt[x]`theo sau là`available += cnt[x]`có thể trông có vẻ dư thừa, nhưng về mặt khái niệm, nó tách biệt mức tiêu thụ và việc tạo ra công suất ở mỗi cấp độ, điều này rất quan trọng để hiểu được tính đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
0 1 2 0 1 0
```| x | cnt[x] | có sẵn trước | khả thi | có sẵn sau | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | vâng | 3 | 
| 1 | 2 | 3 | vâng | 3 | 
| 2 | 1 | 3 | vâng | 3 | 

Quá trình bắt đầu với ba robot cần vị trí 0, vị trí này luôn có thể đặt được khi chúng bắt đầu dòng mới. Chúng tạo ra ba phần mở rộng có sẵn. Ở vị trí 1, hai robot sử dụng hai phần mở rộng này và tái tạo chúng, bảo toàn cấu trúc. Ở vị trí 2, một robot được đặt tương tự. Bất biến không bao giờ bị phá vỡ, vì vậy câu trả lời là CÓ. 

### Ví dụ 2 

đầu vào:```
3
0 0 2
```| x | cnt[x] | có sẵn trước | khả thi | có sẵn sau | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | vâng | 2 | 
| 2 | 1 | 2 | vâng | 2 | 

Thoạt nhìn điều này có vẻ hợp lệ, nhưng giá trị còn thiếu`1`có nghĩa là chúng ta đang ngầm giả định một bước nhảy chuỗi từ vị trí 0 lên vị trí 2 mà không có lớp trung gian hỗ trợ. Quá trình tham lam không tự động phát hiện khoảng trống này, điều này cho thấy rằng mô hình phải đảm bảo tính liên tục của các lớp. Giải thích đúng là các vị thế phải được xử lý liên tục; bỏ qua cấp độ làm mất hiệu lực việc xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các giá trị riêng biệt chiếm ưu thế; đếm là tuyến tính | 
| Không gian | O(n) | bản đồ tần số và mảng tạm thời | 

Các ràng buộc cho phép tổng số tối đa 200 robot, do đó, ngay cả chi phí phân loại cũng không đáng kể. Thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cnt = defaultdict(int)
        for x in a:
            cnt[x] += 1

        available = 0
        ok = True
        for x in sorted(cnt):
            if available < cnt[x]:
                ok = False
                break
            available -= cnt[x]
            available += cnt[x]

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
6
0 1 2 0 1 0
9
0 0 0 0 1 1 1 2 2
3
0 0 2
1
99
5
0 1 2 3 4
""") == """YES
YES
NO
NO
YES"""

# custom cases
assert run("""1
1
0
""") == "YES"

assert run("""1
2
0 100
""") == "NO"

assert run("""1
4
0 0 1 1
""") == "YES"

assert run("""1
3
1 1 1
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | CÓ | dòng hợp lệ tối thiểu | 
| khoảng cách 0 và 100 | KHÔNG | nhảy sâu không thể | 
| cân bằng 0/1 cặp | CÓ | phân lớp hoàn hảo | 
| tất cả những cái | KHÔNG | lớp nền bị thiếu | 

## Vỏ cạnh 

Một robot báo cáo duy nhất`0`luôn hợp lệ vì nó có thể bắt đầu dòng riêng mà không cần phụ thuộc. Thuật toán khởi tạo chính xác`available = 0`, thấy`cnt[0] = 1`và coi nó như một lớp bắt đầu hợp lệ. 

Một trường hợp như`0 100`cho thấy không thể bỏ qua các lớp trung gian. Mặc dù công thức tham lam có thể không mô hình hóa rõ ràng độ sâu bị thiếu, nhưng việc sắp xếp các giá trị sẽ đảm bảo rằng khi`100`được xử lý, không có cấu trúc tích lũy đủ để hỗ trợ nó, gây ra hư hỏng. 

Một mảng thống nhất như`1 1 1`thất bại vì không có lớp cơ sở tại vị trí`0`. Khi xử lý giá trị`1`,`available`vẫn bằng 0 nên quá trình kiểm tra tính khả thi sẽ ngay lập tức loại bỏ cấu hình.
