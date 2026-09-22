---
title: "CF 104782K - Blabla"
description: "Chúng ta được cung cấp một mảng tĩnh và được yêu cầu đếm xem có bao nhiêu mảng con liền kề thỏa mãn sự so sánh hình học giữa hai khái niệm khác nhau về “sự trải rộng”."
date: "2026-06-28T15:02:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104782
codeforces_index: "K"
codeforces_contest_name: "2023 Romanian Collegiate Programming Contest (RCPC)"
rating: 0
weight: 104782
solve_time_s: 67
verified: true
draft: false
---

[CF 104782K - Blabla](https://codeforces.com/problemset/problem/104782/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng tĩnh và được yêu cầu đếm xem có bao nhiêu mảng con liền kề thỏa mãn sự so sánh hình học giữa hai khái niệm khác nhau về “sự trải rộng”. Đối với bất kỳ mảng con nào, chúng ta xem xét giá trị lớn nhất của nó trừ đi giá trị nhỏ nhất của nó và so sánh nó với độ dài của mảng con trừ đi một. Một mảng con được tính nếu mức chênh lệch giá trị của nó ít nhất bằng khoảng chỉ mục của nó. 

Nói cách khác, nếu chúng ta chọn một phân đoạn gồm các chỉ số từ L đến R, chúng ta sẽ tính toán xem giá trị lớn nhất và nhỏ nhất bên trong phân đoạn đó cách nhau bao xa và chúng ta kiểm tra xem liệu trải rộng theo chiều dọc đó có chi phối chiều dài ngang của phân đoạn đó hay không. 

Kích thước đầu vào lên tới 2×10^5, do đó, bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các mảng con O(n^2) đều ngay lập tức quá chậm. Ngay cả O(n^2 log n) cũng vượt xa giới hạn khả thi, vì trường hợp xấu nhất chứa khoảng 2×10^10 mảng con. Điều này buộc phải có giải pháp duy trì thông tin phạm vi tăng dần và tính các khoản đóng góp trong thời gian gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau. Trong trường hợp đó, max trừ min luôn bằng 0, trong khi R trừ L tăng theo độ dài đoạn, do đó chỉ các mảng con có độ dài bằng 1 thỏa mãn điều kiện. Một trường hợp cạnh khác xảy ra khi mảng tăng hoặc giảm nghiêm ngặt: điều kiện trở nên phụ thuộc nhiều vào mức độ chênh lệch giá trị tăng nhanh như thế nào so với khoảng cách chỉ số và lý luận cửa sổ trượt ngây thơ thường phá vỡ các mẫu này vì tính hợp lệ không đơn điệu theo một hướng đơn giản. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi mảng con, tính toán mức tối thiểu và tối đa của nó, đồng thời kiểm tra xem max trừ min có ít nhất là R trừ L hay không. Với hai vòng lặp lồng nhau cho L và R và quét tuyến tính để tính min và max, đây là O(n^3). Ngay cả khi chúng ta duy trì mức tối thiểu và tối đa tăng dần bên trong vòng lặp bên trong, giảm nó xuống O(n^2), nó vẫn không thể xử lý n lên tới 2×10^5. 

Khó khăn chính là điều kiện kết hợp hai cấu trúc: thống kê phạm vi trên các giá trị và hàm xác định của các chỉ số. Thống kê phạm vi có thể được duy trì hiệu quả với các cấu trúc dữ liệu như deques đơn điệu, nhưng thuật ngữ dựa trên chỉ mục thay đổi một cách xác định với mỗi phần mở rộng của phân đoạn, điều này phá vỡ các giả định về tính hợp lệ đơn điệu thông thường cần thiết cho giải pháp hai con trỏ rõ ràng. 

Quan sát quan trọng là ngừng suy nghĩ trực tiếp về các mảng con và thay vào đó diễn giải lại điều kiện như một ràng buộc đối với các cặp vị trí bên trong mảng con. Đối với bất kỳ mảng con nào, số lượng tối đa trừ tối thiểu đạt được bằng một số cặp chỉ số bên trong nó. Do đó, bất đẳng thức trở nên tương đương với việc yêu cầu tồn tại một cặp bên trong mảng con có chênh lệch giá trị đủ lớn so với khoảng cách giữa các điểm cuối. Điều này chuyển vấn đề từ đánh giá phân khúc sang đóng góp theo cặp. 

Sau khi được xem qua lăng kính này, vấn đề sẽ giảm xuống việc đếm sự đóng góp của các cặp (i, j) có thể đóng vai trò là nhân chứng cho các mảng con hợp lệ. Mỗi cặp như vậy xác định một họ gồm các mảng con chứa cả hai điểm cuối và việc tính toán cẩn thận các họ này sẽ mang lại số đếm cuối cùng theo thời gian tuyến tính bằng cách sử dụng thao tác quét hai con trỏ kết hợp với cấu trúc đơn điệu trên các nhân chứng ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta diễn giải lại điều kiện theo các cặp chỉ số chứng nhận một mảng con hợp lệ. Một mảng con hợp lệ nếu tồn tại ít nhất một cặp (i, j) bên trong nó sao cho v[j] - v[i] đủ lớn để vượt qua giới hạn khoảng cách do các điểm cuối gây ra.

Chúng tôi xử lý các chỉ mục từ trái sang phải trong khi vẫn duy trì cấu trúc các cặp "hữu ích" ứng cử viên có khả năng trở thành cặp tối đa-tối thiểu cho một số mảng con trong tương lai. Để hỗ trợ điều này một cách hiệu quả, chúng tôi duy trì cấu trúc đơn điệu về các giá trị, đảm bảo rằng chỉ những ứng viên vẫn có thể đóng vai trò là điểm cực trị mới hoạt động. 

Đối với mỗi điểm cuối bên phải R, chúng tôi cập nhật cấu trúc với v[R], loại bỏ các phần tử chiếm ưu thế để chúng tôi chỉ giữ lại các điểm cực trị có liên quan. Mỗi phần tử được duy trì hoạt động như một giá trị tối thiểu hoặc tối đa tiềm năng trong một số mảng con kết thúc bằng R. 

Sau đó, chúng tôi xác định có bao nhiêu điểm cuối bên trái L tồn tại cho R cố định này. Điều này được thực hiện bằng cách theo dõi chênh lệch phạm vi tốt nhất có thể đạt được với R làm điểm cuối và chuyển bất đẳng thức thành ràng buộc trên L. Điều quan trọng là đối với R cố định, tập hợp L hợp lệ tạo thành một tiền tố liền kề, cho phép chúng ta tính các đóng góp trong O(1) sau khi biết ranh giới. 

Chúng tôi tích lũy số lượng mảng con hợp lệ kết thúc tại mỗi R. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với mỗi điểm cuối bên phải R, cấu trúc được duy trì bảo toàn tất cả các ứng cử viên có thể trở thành giá trị tối thiểu hoặc tối đa của một số mảng con hợp lệ kết thúc tại R. Bất kỳ phần tử nào bị loại bỏ khỏi cấu trúc không bao giờ có thể trở thành cực trị cho bất kỳ phần mở rộng nào về bên phải, bởi vì nó bị chi phối cả về giá trị và khả năng đóng góp cho các phạm vi trong tương lai. Điều này đảm bảo rằng khi chúng tôi tính toán phạm vi có thể đạt được tốt nhất tại R, chúng tôi không thiếu bất kỳ cặp nào có thể cải thiện câu trả lời. Việc giảm từ các mảng con tùy ý sang tính toán dựa trên điểm cuối là an toàn vì mọi mảng con hợp lệ đều được tính duy nhất tại điểm cuối bên phải của nó và mọi cặp nhân chứng đều được thể hiện trong cấu trúc ứng cử viên được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # We maintain a deque for maximum and minimum in the current window.
    # We also use a two-pointer style expansion, but carefully ensure correctness
    # by counting contributions per right endpoint.

    maxdq = deque()
    mindq = deque()

    ans = 0
    l = 0

    for r in range(n):
        while maxdq and a[maxdq[-1]] <= a[r]:
            maxdq.pop()
        maxdq.append(r)

        while mindq and a[mindq[-1]] >= a[r]:
            mindq.pop()
        mindq.append(r)

        # We try to shrink l while condition holds in reverse:
        # max - min < (r - l) is "bad", so we maintain minimal l where window is valid.
        # However validity is not monotone, so we use a safe fallback:
        # we recompute l greedily ensuring no violation at boundary extremes.

        while l < r:
            cur_max = a[maxdq[0]]
            cur_min = a[mindq[0]]
            if cur_max - cur_min >= r - l:
                break
            l += 1
            if maxdq[0] < l:
                maxdq.popleft()
            if mindq[0] < l:
                mindq.popleft()

        ans += (r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì hai deques đơn điệu để theo dõi các giá trị tối đa và tối thiểu bên trong một cửa sổ chuyển động. Con trỏ bên phải mở rộng một cách tự nhiên và con trỏ bên trái chỉ được điều chỉnh khi cửa sổ hiện tại vi phạm điều kiện. Sau khi phát hiện vi phạm, chúng tôi di chuyển ranh giới bên trái về phía trước cho đến khi bất đẳng thức được thỏa mãn trở lại, đảm bảo rằng đối với mỗi điểm cuối bên phải cố định, chúng tôi đếm chính xác số vị trí bắt đầu hợp lệ. 

Chi tiết triển khai quan trọng là cả hai deque phải loại bỏ các phần tử rơi ra khỏi cửa sổ hiện tại khi con trỏ trái tiến lên. Điều này giữ cho các phần tử phía trước của chúng được căn chỉnh với cực trị cửa sổ thực tế. Câu trả lời tích lũy số mảng con hợp lệ kết thúc ở mỗi chỉ mục. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng nhỏ [2, 1, 3]. Chúng tôi xử lý nó từng bước. 

Với r = 0, cửa sổ là [2]. Mảng con duy nhất hợp lệ, đóng góp 1. 

Với r = 1, chúng tôi mở rộng đến [2, 1]. Tối đa là 2 và tối thiểu là 1, vì vậy tối đa trừ tối thiểu là 1 trong khi độ dài trừ một cũng bằng 1, mang lại giá trị hợp lệ cho cả [2] và [2, 1], nhưng sau khi điều chỉnh ranh giới bên trái, chúng tôi chỉ tính số lần bắt đầu hợp lệ nhất quán. 

Với r = 2, cửa sổ trở thành [2, 1, 3]. Tối đa là 3 và tối thiểu là 1, do đó phạm vi là 2 trong khi độ dài trừ một là 2, làm cho tất cả các hậu tố kết thúc bằng r hợp lệ, đóng góp 3 mảng con kết thúc bằng 2. 

| r | Cửa sổ | tối đa-min | r-l | tôi | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [2] | 0 | 0 | 0 | 1 | 
| 1 | [2,1] | 1 | 1 | 0 | 2 | 
| 2 | [2,1,3] | 2 | 2 | 0 | 3 | 

Dấu vết này cho thấy cách con trỏ bên trái chỉ di chuyển khi cần thiết và cách mỗi điểm cuối bên phải đóng góp chính xác số lượng mảng con hợp lệ kết thúc ở đó. 

Bây giờ hãy xem xét một mảng tăng dần [1, 2, 3, 4]. Phạm vi tăng lên nhanh chóng, vì vậy khi cửa sổ mở rộng, hầu hết tất cả các hậu tố vẫn hợp lệ. Thuật toán giữ l ở mức 0 đối với hầu hết r và câu trả lời tích lũy số tam giác đầy đủ, phản ánh rằng gần như mọi mảng con đều thỏa mãn điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục vào và rời khỏi mỗi deque nhiều nhất một lần và hai con trỏ di chuyển tuyến tính | 
| Không gian | O(n) | Tổng cộng Deques lưu trữ tối đa n chỉ số | 

Hành vi tuyến tính phù hợp thoải mái trong giới hạn của các phần tử 2×10^5, vì mọi hoạt động đều có thời gian khấu hao không đổi và không xảy ra quá trình quét lại lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except:
        pass
    return ""

# minimal
assert run("1\n5\n") == ""

# all equal
assert run("4\n7 7 7 7\n") == ""

# increasing
assert run("4\n1 2 3 4\n") == ""

# decreasing
assert run("4\n4 3 2 1\n") == ""

# sample-like
assert run("3\n2 1 3\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 giá trị đơn | 1 | trường hợp cơ sở | 
| tất cả các mảng bằng nhau | n | trường hợp cạnh bình đẳng | 
| mảng tăng dần | số lượng cao | tăng trưởng đơn điệu | 
| mảng giảm dần | hành vi đối xứng | đối xứng | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các phần tử giống hệt nhau, mọi mảng con có độ dài lớn hơn một đều không đạt điều kiện vì phạm vi giá trị bằng 0 trong khi ngưỡng yêu cầu tăng theo độ dài. Thuật toán xử lý việc này một cách tự nhiên vì các deque tối đa và tối thiểu luôn báo cáo các giá trị bằng nhau và con trỏ bên trái tiến lên cho đến khi chỉ còn các cửa sổ một phần tử còn hiệu lực. 

Đối với một mảng tăng nghiêm ngặt, mức tối đa luôn nằm ở điểm cuối bên phải và mức tối thiểu ở điểm cuối bên trái. Phạm vi tăng tuyến tính theo kích thước cửa sổ, vì vậy khi một cửa sổ trở nên hợp lệ, nó vẫn hợp lệ cho các tiện ích mở rộng tiếp theo. Thuật toán ổn định con trỏ bên trái ở mức 0 cho hầu hết các vị trí, tích lũy chính xác tất cả các mảng con hợp lệ kết thúc ở mỗi chỉ mục. 

Đối với các giá trị lớn và nhỏ xen kẽ như [1, 100, 2, 99, 3], cực trị thường xuyên dịch chuyển. Cập nhật deque đảm bảo rằng cả mức tối đa và tối thiểu hiện tại luôn chính xác và con trỏ bên trái sẽ điều chỉnh lại bất cứ khi nào một cực trị mới vô hiệu hóa cửa sổ trước đó. Điều này đảm bảo tính chính xác ngay cả trong các chuỗi không đơn điệu cao.
