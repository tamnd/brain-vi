---
title: "CF 1481C - Sơn hàng rào"
description: "Chúng ta có một hàng rào gồm n tấm ván, ban đầu mỗi tấm được sơn màu từ a1 đến an. Mục tiêu của chúng tôi là sơn lại hàng rào sao cho cấu hình màu cuối cùng khớp với mảng mục tiêu b1 đến bn."
date: "2026-06-10T23:35:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1481
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 699 (Div. 2)"
rating: 1600
weight: 1481
solve_time_s: 150
verified: false
draft: false
---

[CF 1481C - Vẽ hàng rào](https://codeforces.com/problemset/problem/1481/C) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng rào bao gồm`n`tấm ván, ban đầu mỗi tấm được sơn một màu từ`a_1`ĐẾN`a_n`. Mục tiêu của chúng tôi là sơn lại hàng rào sao cho cấu hình màu cuối cùng phù hợp với mảng mục tiêu`b_1`ĐẾN`b_n`. Chúng tôi có`m`họa sĩ, mỗi người đến theo thứ tự và mỗi người có thể vẽ chính xác một tấm ván với một màu cụ thể`c_j`. Chúng ta không thể từ chối một họa sĩ, nhưng chúng ta có thể chọn tấm ván họ vẽ. Nhiệm vụ là quyết định xem có thể đạt được màu sắc mong muốn hay không, và nếu có, hãy giao một tấm ván cho mỗi họa sĩ theo thứ tự nhất định. 

Những ràng buộc cho phép`n`Và`m`lên đến`10^5`cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi`10^5`. Điều này cho thấy chúng ta cần một giải pháp chạy gần như theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Một cách tiếp cận mạnh mẽ để kiểm tra tất cả các bài tập ván có thể có cho mỗi họa sĩ sẽ quá chậm vì nó có thể yêu cầu`O(n^m)`hoạt động. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó màu của họa sĩ cuối cùng không khớp với bất kỳ tấm ván nào cần sơn lại hoặc khi nhiều họa sĩ có thể sơn cùng một tấm ván nhưng chúng tôi vẫn phải thực hiện tất cả các thay đổi cần thiết. Ví dụ: nếu tất cả các tấm ván đã có màu mong muốn nhưng họa sĩ cuối cùng có màu mà hiện tại không có tấm ván nào cần, thì một cách tiếp cận ngây thơ có thể không chỉ định được một tấm ván hợp lệ. 

Ví dụ, nếu`a = [1, 1]`,`b = [1, 2]`, Và`c = [2]`, cách duy nhất để đạt được mục tiêu là người họa sĩ duy nhất sơn tấm ván thứ hai. Nếu chọn nhầm tấm ván đầu tiên, chúng ta sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi nhiệm vụ ván có thể có cho mỗi họa sĩ. Đối với mỗi họa sĩ, chúng ta có thể thử sơn mọi tấm ván phù hợp với màu sắc của họ và kiểm tra đệ quy họa sĩ tiếp theo. Điều này đúng vì nó khám phá mọi khả năng, nhưng nó khó thực hiện đối với số lượng lớn`n`Và`m`. Ngay cả đối với kích thước vừa phải như`n = m = 10^5`, điều này sẽ đòi hỏi các hoạt động thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng chỉ những tấm ván ở nơi`a_i != b_i`cần phải sơn lại. Chúng ta có thể duy trì ánh xạ từ mỗi màu đến danh sách các tấm ván yêu cầu màu đó. Họa sĩ đến với một màu sắc cụ thể`c_j`lý tưởng nhất là nên sơn một trong những tấm ván còn lại cần`c_j`. Nếu không có tấm ván như vậy, chúng ta có thể sơn bất kỳ tấm ván nào đã có màu`c_j`để đảm bảo thợ sơn được sử dụng. Tấm ván của người họa sĩ cuối cùng rất quan trọng vì nếu chúng ta không chọn được tấm ván hợp lệ thì chúng ta không thể thỏa mãn các ràng buộc. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng một từ điển ánh xạ màu sắc vào danh sách các tấm ván vẫn cần màu đó. Chúng tôi lặp lại các họa sĩ theo thứ tự và chỉ định cho họ một tấm ván dựa trên ánh xạ này, xử lý đặc biệt họa sĩ cuối cùng nếu cần thiết. Điều này làm giảm vấn đề từ bùng nổ tổ hợp đến xử lý thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy tạo một từ điển ánh xạ màu tới các chỉ mục của các tấm ván vẫn cần màu đó (`a_i != b_i`). Điều này cho phép chúng ta nhanh chóng tìm ra tấm ván nào có thể được sơn bởi một họa sĩ nhất định. 
2. Xác định màu của họa sĩ cuối cùng`c_m`. Kiểm tra xem có ít nhất một tấm ván trong mục tiêu không`b`với màu này. Nếu không, xuất ngay "NO" vì người họa sĩ cuối cùng phải sơn tấm ván màu này. 
3. Đối với các họa sĩ từ người đầu tiên đến người áp chót, nếu có tấm ván nào cần màu của họ, hãy giao cho họ và xóa nó khỏi từ điển. Nếu không có tấm ván nào cần màu, hãy chỉ định bất kỳ tấm ván nào có màu của họa sĩ cuối cùng. Điều này đảm bảo rằng mỗi họa sĩ sẽ vẽ một thứ gì đó trong khi để lại cho họa sĩ cuối cùng một tấm ván hợp lệ. 
4. Chỉ định họa sĩ cuối cùng cho bất kỳ tấm ván nào có màu sắc của họ. Đến bước 3, chúng tôi đảm bảo có ít nhất một tấm ván như vậy. 
5. Sau khi chỉ định tất cả thợ sơn, hãy kiểm tra xem còn tấm ván nào cần sơn lại không. Nếu vậy, xuất ra "KHÔNG". Ngược lại, xuất "CÓ" và danh sách các chỉ số của ván. 

Tại sao nó hoạt động: Điều bất biến là mỗi họa sĩ sẽ sơn một tấm ván vẫn cần màu của họ hoặc một phương án dự phòng an toàn (tấm ván có màu của họa sĩ cuối cùng). Người họa sĩ cuối cùng được đảm bảo có một tấm ván hợp lệ vì chúng tôi đã kiểm tra sự tồn tại của nó. Tất cả các lần sơn lại cần thiết đều được thực hiện, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        c = list(map(int, input().split()))
        
        need = {}
        for i in range(n):
            if a[i] != b[i]:
                need.setdefault(b[i], []).append(i)
        
        last_color = c[-1]
        if last_color in need:
            last_plank = need[last_color][-1]
        else:
            found = -1
            for i in range(n):
                if b[i] == last_color:
                    found = i
                    break
            if found == -1:
                print("NO")
                continue
            last_plank = found
        
        res = [0] * m
        res[-1] = last_plank
        for j in range(m-1):
            color = c[j]
            if color in need and need[color]:
                res[j] = need[color].pop()
            else:
                res[j] = last_plank
        
        impossible = any(need[color] for color in need)
        if impossible:
            print("NO")
        else:
            print("YES")
            print(' '.join(str(x+1) for x in res))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi tạo ánh xạ màu cho các tấm ván vẫn cần sơn lại. Chúng tôi xử lý họa sĩ cuối cùng trước vì màu của họ phải xuất hiện trên ít nhất một tấm ván. Những người họa sĩ còn lại được giao những tấm ván một cách tham lam: hoặc là tấm ván cần màu sắc của họ hoặc tấm ván dự phòng. Kiểm tra cuối cùng đảm bảo không có yêu cầu sơn lại nào bị bỏ sót. Các chỉ số được chuyển đổi thành đầu ra dựa trên 1. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
1 1
1
1
1
```| tôi | một [tôi] | b[i] | cần từ điển | c[j] | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | {} | 1 | 1 | 

Hàng rào đã đúng rồi. Họa sĩ duy nhất có thể sơn tấm ván duy nhất bằng màu của họ. Đầu ra là CÓ. 

**Đầu vào mẫu 2**```
5 2
1 2 2 1 1
1 2 2 1 1
1 2
```| tôi | một [tôi] | b[i] | cần từ điển | c[j] | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | {} | 1 | 2 | 
| 1 | 2 | 2 | {} | 2 | 2 | 

Không cần sơn lại. Cả hai họa sĩ đều sơn tấm ván với màu sắc phù hợp với chúng. Đầu ra là CÓ. 

Những dấu vết này xác nhận rằng thuật toán xử lý chính xác các trường hợp không cần sơn lại và đảm bảo tồn tại màu của họa sĩ cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Xây dựng từ điển cần thiết là O(n), gán các họa sĩ là O(m) | 
| Không gian | O(n) | Lưu trữ từ điển cần thiết và kết quả | 

Độ phức tạp tuyến tính phù hợp với các ràng buộc lên tới`10^5`ván và họa sĩ trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as io_module
    out = io_module.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n1 1\n1\n1\n1\n5 2\n1 2 2 1 1\n1 2 2 1 1\n1 2\n3 3\n2 2 2\n2 2 2\n2 3 2\n10 5\n7 3 2 1 7 9 4 2 7 9\n9 9 2 1 4 9 4 2 3 9\n9 9 7 4 3\n5 2\n1 2 2 1 1\n1 2 2 1 1\n3 3\n6 4\n3 4 2 4 1 2\n2 3 1 3 1 1\n2 2 3 4") == "YES\n1\nYES\n2 2\nYES\n1 1 1\nYES\n2 1 9 5 9\nNO\nNO"

# custom cases
assert run("1\n2 2\n1 2\n2 1\n1 2") == "YES\n2 1", "swap colors"
assert run("
```
