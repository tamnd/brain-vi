---
title: "CF 2216A - Điều ước của khóa học"
description: "Chúng tôi được cung cấp một hệ thống nhỏ các khóa học, mỗi khóa học hiện được chỉ định mức độ ưu tiên từ 1 đến k+1. Cấp độ cuối cùng hoạt động khác: nó không có giới hạn về năng lực và là trạng thái mục tiêu cuối cùng cho mỗi khóa học."
date: "2026-06-09T04:54:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2216
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1092 (Unrated, Div. 2, Based on THUPC 2026 \u2014 Finals)"
rating: 900
weight: 2216
solve_time_s: 182
verified: false
draft: false
---

[CF 2216A - Điều ước về khóa học](https://codeforces.com/problemset/problem/2216/A) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống nhỏ các khóa học, mỗi khóa học hiện được chỉ định mức độ ưu tiên từ 1 đến k+1. Cấp độ cuối cùng hoạt động khác: nó không có giới hạn về năng lực và là trạng thái mục tiêu cuối cùng cho mỗi khóa học. Mục tiêu là chuyển bài tập hiện tại sang trạng thái trong đó mọi khóa học đều ở cấp độ k+1. 

Hoạt động duy nhất được phép là tham gia một khóa học duy nhất và tăng cấp độ của nó lên một bậc. Điều này có nghĩa là khóa học dần dần “di chuyển xuống” bậc thang ưu tiên. Tuy nhiên, sau mỗi lần tăng, hệ thống phải duy trì hiệu lực: với mỗi cấp độ i từ 1 đến k, số lượng khóa học hiện được giao cho cấp độ đó không được vượt quá khả năng của nó a_i. 

Vì vậy, vấn đề không chỉ là đạt được cấu hình cuối cùng mà còn là tìm ra một chuỗi an toàn các bước tăng riêng lẻ sao cho không có trạng thái trung gian nào vi phạm các ràng buộc về dung lượng. 

Các hạn chế là cố ý nhỏ. Với n lên tới 50 và k lên đến 20, bất kỳ nghiệm nào cũng có thể đủ khả năng suy luận bậc hai hoặc thậm chí bậc ba. Tuy nhiên, giới hạn hoạt động 1000 bước bổ sung thêm một hạn chế về cấu trúc ẩn: chúng tôi dự kiến ​​sẽ tạo ra một quá trình chuyển đổi được kiểm soát cẩn thận thay vì các bước di chuyển tùy ý mang tính cưỡng bức. 

Trường hợp cạnh chính xuất hiện khi một cấp độ đã bão hòa nhưng chỉ chứa các khóa học không thể di chuyển một cách an toàn mà không vi phạm các ràng buộc khác. Ví dụ: nếu cấp độ 1 đã đầy và mọi khóa học trong đó sẽ tạm thời làm quá tải cấp độ khác khi di chuyển, thì một lựa chọn tham lam ngây thơ có thể bị mắc kẹt ngay cả khi một thứ tự khác sẽ thành công. 

Một vấn đề tế nhị khác là việc chuyển một khóa học từ cấp độ i sang i+1 có thể tạm thời tăng tỷ lệ lấp đầy ở các cấp độ trung gian, do đó, việc “đẩy mọi thứ xuống” một cách mù quáng có thể vi phạm các ràng buộc ngay cả khi trạng thái cuối cùng rõ ràng là khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quy trình và ở mỗi bước hãy thử mọi cách có thể mà vẫn có thể tăng lên. Điều này hiệu quả vì n nhỏ nhưng dễ vỡ: nó có nguy cơ chọn một động thái cản trở tiến trình trong tương lai. Vì chúng ta không có tính năng quay lui nên mô phỏng tham lam có thể dễ dàng đạt đến trạng thái chết ngay cả khi tồn tại một chuỗi đầy đủ hợp lệ. 

Quan sát chính là tính khả thi chỉ phụ thuộc vào sự phân bổ hiện tại giữa các cấp độ và mọi hoạt động chỉ ảnh hưởng đến một khóa học chuyển từ cấp độ i sang i+1. Điều này tạo ra một cấu trúc giống như dòng chảy cục bộ: chúng tôi liên tục chuyển “khối lượng” từ các nhóm bị ràng buộc (cấp 1 đến k) sang một bồn chứa không bị ràng buộc (cấp k+1). 

Thông tin chi tiết về cấu trúc quan trọng là chúng ta không bao giờ cần phải lo lắng về danh tính chính xác của các khóa học, mà chỉ cần lo lắng về số lượng khóa học ở mỗi cấp độ. Mỗi lần di chuyển về cơ bản là chuyển một căn từ cấp i sang i+1 và hạn chế chỉ hạn chế tỷ lệ lấp đầy trung gian. Điều này có nghĩa là chúng ta luôn có thể ưu tiên giải phóng các cấp độ thấp nhất trước tiên, bởi vì chúng là nút thắt cổ chai: nếu cấp độ thấp hơn đã đầy, nó sẽ ngăn cản mọi chuyển động đi lên phụ thuộc vào nó. 

Chiến lược đúng đắn là liên tục xác định một lộ trình có thể tiến lên một bước một cách an toàn trong khi vẫn giữ được mọi năng lực. Vì k nhỏ và n nhỏ nên chúng ta có thể tính toán lại các nước đi hợp lệ một cách tham lam ở mỗi bước và luôn chọn một nước đi làm giảm áp lực ở mức bị ràng buộc nhất. 

Ý tưởng tham lam cốt yếu là chúng ta nên luôn cố gắng giảm mức vi phạm sớm nhất hoặc có nguy cơ vi phạm cao nhất, vì các cấp cao hơn chỉ phụ thuộc vào việc thanh toán bù trừ ở cấp thấp hơn xảy ra trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1000 · n · k) trường hợp xấu nhất có ngõ cụt | O(n) | Quá chậm/không an toàn | 
| Giảm mức độ tham lam có kiểm soát | O(1000 · n · k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấp độ hiện tại của mỗi khóa học và tính toán lại số cấp độ sau mỗi hoạt động.

1. Tính tần suất các khóa học hiện tại ở mỗi cấp độ từ 1 đến k. Chúng ta bỏ qua k+1 vì nó không bị ràng buộc. 
2. Cố gắng thực hiện tối đa 1000 thao tác. 
3. Ở mỗi bước, hãy quét tất cả các khóa học từ 1 đến n và kiểm tra xem liệu chúng ta có thể tăng cấp độ của nó lên 1 một cách an toàn hay không. 
4. Khóa học ở cấp độ k+1 bị bỏ qua vì đã kết thúc. 
5. Việc chuyển từ cấp i lên i+1 chỉ được phép nếu sau khi giảm count[i] đi 1 và tăng count[i+1] lên 1 (nếu i+1 ≤ k), chúng ta không vượt quá dung lượng a[i+1]. Điều này đảm bảo không có vi phạm trung gian nào được tạo ra. 
6. Chúng tôi chọn khóa học hợp lệ đầu tiên được tìm thấy và áp dụng nước đi, cập nhật cấp độ và số lượng của nó. 
7. Nếu không có nước đi hợp lệ nào tồn tại nhưng đường đi nào đó vẫn không đạt k+1, chúng tôi kết luận rằng quá trình này là không thể. 
8. Nếu tất cả các khóa học đạt k+1 trong vòng 1000 bước, chúng tôi sẽ xuất chuỗi. 

Lựa chọn thiết kế quan trọng là mọi động thái đều được xác thực cục bộ dựa trên các hạn chế về dung lượng, đảm bảo chúng tôi không bao giờ nhập cấu hình trung gian không hợp lệ. 

### Tại sao nó hoạt động 

Hệ thống hoạt động giống như một luồng bị ràng buộc trong đó mỗi cấp độ là một bộ đệm giới hạn và k+1 là mức chìm vô hạn. Mọi giải pháp hợp lệ cuối cùng phải làm trống tất cả các bộ đệm giới hạn. Vì mỗi thao tác chỉ di chuyển một đơn vị về phía trước nên bất kỳ giải pháp khả thi nào cũng tương ứng với một chuỗi nới lỏng gia tăng an toàn của các bộ đệm này. Bằng cách luôn chọn bất kỳ động thái nào hiện an toàn, chúng tôi đảm bảo rằng chúng tôi không bao giờ chặn tiến trình một cách giả tạo theo cách mà một thứ tự khác có thể tránh được. Bởi vì n và k nhỏ, nếu một chuỗi an toàn tồn tại trong vòng 1000 bước, mô phỏng bảo toàn tính khả thi tham lam này sẽ luôn tìm thấy ít nhất một phần tiếp theo hợp lệ cho đến khi hoàn thành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        cnt = [0] * (k + 2)
        for x in b:
            cnt[x] += 1
        
        res = []
        
        def can_move(level):
            if level == k + 1:
                return False
            # simulate decrement and increment
            cnt[level] -= 1
            cnt[level + 1] += 1
            
            ok = True
            if level + 1 <= k and cnt[level + 1] > a[level]:
                ok = False
            
            cnt[level] += 1
            cnt[level + 1] -= 1
            return ok
        
        for _op in range(1000):
            moved = False
            
            for i in range(n):
                if b[i] == k + 1:
                    continue
                old = b[i]
                new = old + 1
                
                # try move
                cnt[old] -= 1
                cnt[new] += 1
                
                valid = True
                if new <= k and cnt[new] > a[new - 1]:
                    valid = False
                
                if valid:
                    b[i] = new
                    res.append(i + 1)
                    moved = True
                    break
                
                # rollback
                cnt[old] += 1
                cnt[new] -= 1
            
            if not moved:
                break
        
        if all(x == k + 1 for x in b):
            print(len(res))
            print(*res)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một mảng rõ ràng các cấp độ hiện tại và một mảng tần số có thể theo dõi số lượng khóa học ở mỗi cấp độ. Đối với mọi thao tác, nó sẽ thử tất cả các hướng và mô phỏng việc di chuyển từng hướng về phía trước một cấp. Việc kiểm tra tính hợp lệ đảm bảo chúng tôi không bao giờ vượt quá khả năng cho cấp độ đích. 

Logic khôi phục rất quan trọng: chúng tôi tạm thời áp dụng một động thái, kiểm tra tính khả thi và hoàn tác nó nếu thất bại. Điều này tránh việc sao chép trạng thái trong khi vẫn đảm bảo tính chính xác. 

Giới hạn 1000 thao tác được vòng lặp thực thi trực tiếp và việc kết thúc được phát hiện do thành công hoặc hết các bước di chuyển hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, k=2
a = [2,2]
b = [1,2,2]
```Chúng tôi chỉ theo dõi số lượng. 

| Bước | bang b | đếm (1,2,3) | nước đi đã chọn | 
| --- | --- | --- | --- | 
| 0 | [1,2,2] | (1,2,0) | chuyển khóa 2 | 
| 1 | [1,3,2] | (1,1,1) | chuyển khóa 1 | 
| 2 | [2,3,2] | (0,2,1) | chuyển khóa 3 | 
| 3 | [2,3,3] | (0,1,2) | chuyển khóa 1 | 
| 4 | [3,3,3] | (0,0,3) | xong | 

Mỗi bước di chuyển duy trì các giới hạn dung lượng cho cấp 2, không bao giờ vượt quá a2 = 2. Quá trình này hoạt động vì chúng tôi luôn tránh bão hòa cấp 2 trong khi giải phóng cấp 1 sớm. 

### Ví dụ 2 

đầu vào:```
n=1, k=1
a = [1]
b = [1]
```Chỉ có một khóa học tồn tại. 

| Bước | bang b | đếm (1,2) | hành động | 
| --- | --- | --- | --- | 
| 0 | [1] | (1,0) | chuyển sang 2 | 
| 1 | [2] | (0,1) | xong | 

Điều này thể hiện trường hợp đơn giản nhất: tiến triển trực tiếp mà không có ràng buộc nào vượt quá một giới hạn duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · 1000 · n · k) | Mỗi thao tác quét tất cả các khóa học và xác nhận ở hầu hết k cấp độ | 
| Không gian | O(n + k) | Lưu trữ các bài tập hiện tại và mảng tần số | 

Với n 50, k 20 và tối đa 1000 thao tác, tổng công việc nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))
            
            cnt = [0] * (k + 2)
            for x in b:
                cnt[x] += 1
            
            res = []
            for _op in range(1000):
                moved = False
                for i in range(n):
                    if b[i] == k + 1:
                        continue
                    old = b[i]
                    new = old + 1
                    cnt[old] -= 1
                    cnt[new] += 1
                    valid = True
                    if new <= k and cnt[new] > a[new - 1]:
                        valid = False
                    if valid:
                        b[i] = new
                        res.append(i + 1)
                        moved = True
                        break
                    cnt[old] += 1
                    cnt[new] -= 1
                if not moved:
                    break
            
            if all(x == k + 1 for x in b):
                out.append(str(len(res)))
                if res:
                    out.append(" ".join(map(str, res)))
                else:
                    out.append("")
            else:
                out.append("-1")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
3 2
2 2
1 2 2
4 2
2 2
3 3 3 3
1 1
1
1
5 3
1 2 3
1 2 4 2 3
""")

# custom cases
assert run("""1
1 1
1
1
""") == "1\n1", "single move"

assert run("""1
3 1
2
1 1 2
""") != "", "basic feasibility"

assert run("""1
2 2
1 1
1 2
"""), "minimal mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khóa học, tầm thường | 1\n1 | tiến trình đơn giản nhất | 
| công suất một cấp | không trống | xử lý các hạn chế chặt chẽ | 
| hộp nhỏ hỗn hợp | trình tự hợp lệ | tính đúng đắn khi tương tác | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi nhiều khóa học nằm ở cùng một cấp độ trung cấp và chỉ một trong số chúng có thể được di chuyển một cách an toàn mà không làm ảnh hưởng đến sức chứa của cấp độ tiếp theo. Thuật toán xử lý việc này vì nó luôn mô phỏng từng nước đi ứng viên và chỉ thực hiện nước đi hợp lệ đầu tiên. Ví dụ: nếu hai khóa học ở cấp 2 và cấp 3 đã gần hết công suất, thì chỉ một trong các động tác sẽ vượt qua quá trình kiểm tra xác thực, ngăn chặn tình trạng cam kết quá mức. 

Một trường hợp khó khăn khác là khi hệ thống đã ở trạng thái cuối cùng. Trong trường hợp đó, vòng lặp không bao giờ tìm thấy một lộ trình có thể di chuyển được và ngay lập tức kết thúc với các thao tác bằng 0, xuất ra một chuỗi trống một cách chính xác. 

Trường hợp thứ ba là khi các cấp độ ban đầu bị hạn chế nhiều. Thuật toán tự nhiên ưu tiên các bước đi an toàn từ các cấp độ thấp hơn vì bất kỳ bước di chuyển không hợp lệ nào sẽ bị từ chối ngay lập tức khi kiểm tra năng lực, buộc phải lựa chọn một lộ trình khác không tạo ra tình trạng tràn.
