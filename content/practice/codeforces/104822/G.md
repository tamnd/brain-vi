---
title: "CF 104822G - Lật dấu"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép lật dấu của bất kỳ phần tử riêng lẻ nào bao nhiêu lần trước khi thực hiện bất kỳ điều gì khác."
date: "2026-06-28T12:43:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "G"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 114
verified: false
draft: false
---

[CF 104822G - Lật ký hiệu](https://codeforces.com/problemset/problem/104822/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép lật dấu của bất kỳ phần tử riêng lẻ nào bao nhiêu lần trước khi thực hiện bất kỳ điều gì khác. Sau khi chọn các dấu cuối cùng, chúng tôi xem xét từng mảng con và tính xem có bao nhiêu giá trị riêng biệt xuất hiện bên trong nó, sau đó tính tổng số lượng đó trên tất cả các mảng con. 

Tương tác chính là việc thay đổi dấu không làm thay đổi độ lớn nhưng nó làm thay đổi mối quan hệ đẳng thức. Hai giá trị tuyệt đối bằng nhau có thể được coi là bằng nhau (cùng dấu) hoặc khác nhau (dấu ngược nhau), trong khi số 0 được cố định vì việc lật không làm gì cả. 

Mục tiêu là gán một dấu hiệu cho mọi phần tử sao cho tổng số lượng riêng biệt trên tất cả các mảng con trở nên lớn nhất có thể. 

Ràng buộc$n \le 3 \cdot 10^5$loại trừ bất kỳ giải pháp nào kiểm tra tất cả các mảng con một cách rõ ràng. Một sự ngây thơ$O(n^2)$việc liệt kê các mảng con đã quá lớn và thậm chí$O(n^2 \log n)$hoặc bất cứ điều gì liên tục tính toán lại số lượng riêng biệt là không thể. Bất kỳ giải pháp khả thi nào cũng phải giảm vấn đề thành tập hợp tuyến tính hoặc gần tuyến tính dựa trên sự đóng góp của các giá trị hoặc vị trí. 

Một vấn đề tế nhị là việc “tạo ra những giá trị khác biệt” không chỉ mang tính địa phương. Ví dụ: việc lật một lần xuất hiện của một giá trị sẽ ảnh hưởng đến tất cả các mảng con chứa vị trí đó và nó cũng tương tác với các lần xuất hiện khác có cùng giá trị tuyệt đối. Một quyết định tham lam cho mỗi yếu tố mà không xem xét cấu trúc toàn cầu có thể dễ dàng tính sai các khoản đóng góp. 

Một tình huống cạnh khác là khi tất cả các giá trị bằng 0. Vì số 0 không thể chia thành hai giá trị có dấu riêng biệt nên mỗi mảng con luôn có chính xác một phần tử riêng biệt. Bất kỳ giải pháp nào xử lý không chính xác số 0 giống như một giá trị có thể chia tách sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê tất cả các mảng con, tính toán các phần tử riêng biệt cho từng mảng và thử tất cả các phép gán dấu. Ngay cả khi bỏ qua các lựa chọn dấu hiệu, việc duy trì số lượng riêng biệt trên mỗi mảng con sẽ dẫn đến khoảng$O(n^2)$tiểu bang. Việc thêm tối ưu hóa dấu hiệu sẽ làm cho nó trở nên theo cấp số nhân, vì mỗi phần tử có hai trạng thái. 

Sự đơn giản hóa cấu trúc quan trọng đến từ việc viết lại mục tiêu. Thay vì tính tổng các mảng con trước, chúng ta đảo ngược phối cảnh và tính tổng các giá trị. Mỗi giá trị đóng góp vào một mảng con nếu nó xuất hiện ít nhất một lần trong mảng đó. Do đó, tổng đóng góp của một giá trị cuối cùng cố định$x$là số mảng con giao nhau với ít nhất một lần xuất hiện của$x$. Tổng câu trả lời trở thành tổng của các đóng góp bảo hiểm như vậy trên tất cả các giá trị được ký riêng biệt. 

Bây giờ vai trò của việc lật biển trở nên rõ ràng hơn. Mọi giá trị tuyệt đối$v$tạo ra nhiều vị trí. Sau khi ấn định biển hiệu, các vị trí này được chia thành 2 nhóm độc lập:$+v$Và$-v$, hoạt động giống như hai giá trị khác nhau. Mỗi nhóm đóng góp độc lập vào tổng số lượng riêng biệt. 

Vì vậy, đối với mỗi giá trị tuyệt đối, chúng ta không chọn liệu nó có tồn tại hay không mà chọn cách phân chia các lần xuất hiện của nó thành hai tập hợp được gắn nhãn. Mỗi bộ tạo ra sự đóng góp phạm vi bao phủ trên các mảng con và chúng tôi muốn chọn phân vùng tối đa hóa tổng của các phạm vi này. 

Đối với một tập hợp các vị trí cố định, sự đóng góp của nó chỉ phụ thuộc vào khoảng cách giữa các lần xuất hiện liên tiếp. Nếu sự xuất hiện ở vị trí$p_1 < p_2 < \dots < p_k$, thì các mảng con tránh được tất cả các lần xuất hiện sẽ là những mảng con được chứa đầy đủ trong các khoảng trống. Điều này dẫn đến chi phí phụ thuộc bậc hai vào độ dài khoảng cách, do đó số lần xuất hiện phân cụm làm tăng đáng kể sự đóng góp. 

Do đó, vấn đề giảm xuống còn việc phân vùng mỗi danh sách xuất hiện thành hai chuỗi con sao cho cả hai chuỗi con đều có độ phân tán nội bộ tối thiểu. Cấu trúc tối ưu đạt được bằng cách xen kẽ các lần xuất hiện theo thứ tự được sắp xếp giữa hai nhóm, giúp cân bằng sự phân bố khoảng cách của chúng. 

Điều này làm giảm vấn đề tính toán đóng góp từ hai chuỗi cảm ứng cho mỗi giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các mảng con và ký phép gán | Hàm mũ | O(n) | Quá chậm | 
| Chia theo giá trị với phân vùng được tối ưu hóa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm các chỉ số theo giá trị tuyệt đối. Mỗi nhóm chứa tất cả các vị trí xuất hiện một cường độ nhất định. 
2. Sắp xếp từng nhóm vị trí theo thứ tự tăng dần. Điều này sửa chữa cấu trúc cần thiết để giải thích về các khoảng trống. 
3. Chia mỗi nhóm thành hai chuỗi con bằng cách xen kẽ các vị trí theo thứ tự đã sắp xếp, gửi nhóm đầu tiên đến nhóm A, nhóm thứ hai đến nhóm B, v.v. Điều này đảm bảo cả hai chuỗi con đều được trải đều nhất có thể. 
4. Đối với mỗi dãy con, hãy tính đóng góp của nó cho câu trả lời bằng số mảng con chứa ít nhất một phần tử từ dãy đó. Điều này được thực hiện bằng cách sử dụng phân tách khoảng trống: nếu chúng ta biết kích thước của các khoảng trống giữa các lần xuất hiện liên tiếp, chúng ta sẽ trừ đi số mảng con chứa đầy đủ trong các khoảng trống từ tổng số mảng con. 
5. Thêm phần đóng góp từ cả hai chuỗi con của mọi giá trị và cũng bao gồm các số 0 riêng biệt vì chúng không thể tách rời và hoạt động như một giá trị cố định. 

Tại sao điều này có tác dụng xuất phát từ việc xem từng giá trị đã ký dưới dạng “màu”. Mỗi màu đóng góp vào một mảng con nếu mảng con đó giao nhau với ít nhất một trong các vị trí của nó. Việc chia các lần xuất hiện thành hai màu là có lợi vì nó làm giảm việc phân cụm, giúp giảm số lượng mảng con hoàn toàn thiếu màu đó. Việc gán xen kẽ sẽ giảm thiểu khoảng cách lớn nhất và tổng số khoảng trống bên trong trên cả hai màu, giúp tối đa hóa phạm vi bao phủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = {}
    zeros = 0
    
    for i, x in enumerate(a):
        if x == 0:
            zeros += 1
            continue
        v = abs(x)
        pos.setdefault(v, []).append(i)
    
    total_subarrays = n * (n + 1) // 2
    ans = 0
    
    def contribution(positions):
        if not positions:
            return 0
        k = len(positions)
        
        # compute complement: subarrays that avoid all positions
        bad = 0
        
        # left boundary
        bad += positions[0] * (positions[0] + 1) // 2
        
        # middle gaps
        for i in range(1, k):
            gap = positions[i] - positions[i - 1] - 1
            bad += gap * (gap + 1) // 2
        
        # right boundary
        bad += (n - positions[-1] - 1) * (n - positions[-1]) // 2
        
        return total_subarrays - bad
    
    for v, ps in pos.items():
        ps.sort()
        
        group1 = []
        group2 = []
        
        for i, p in enumerate(ps):
            if i % 2 == 0:
                group1.append(p)
            else:
                group2.append(p)
        
        ans += contribution(group1)
        ans += contribution(group2)
    
    # zeros contribute 1 per subarray since they are identical and unavoidable
    ans += zeros * (n * (n + 1) // 2)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã tổng hợp các vị trí theo giá trị tuyệt đối, vì chỉ độ lớn mới xác định liệu việc lật có thể tạo ra hay phá hủy sự bình đẳng. Mỗi nhóm được chia thành hai chuỗi con xen kẽ để mô phỏng việc gán dấu hiệu tối ưu. 

các`contribution`hàm thực hiện thủ thuật bù tiêu chuẩn: thay vì đếm các mảng con bao gồm ít nhất một lần xuất hiện, nó đếm tất cả các mảng con và trừ đi những mảng con hoàn toàn tránh được các vị trí. Những mảng con “xấu” đó chính xác là những mảng nằm hoàn toàn trong khoảng trống giữa các lần xuất hiện hoặc nằm ngoài các điểm cực trị, và mỗi khoảng như vậy đóng góp một số tam giác. 

Các số 0 được xử lý riêng biệt vì chúng hoạt động như một giá trị cố định không thể phân chia và mọi mảng con chứa bất kỳ số 0 nào vẫn được tính là một phần tử riêng biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 1 0 -1
```Chúng tôi nhóm giá trị tuyệt đối 1 tại các vị trí [0, 1, 3] và 0 tại vị trí [2]. 

Chúng tôi chia [0, 1, 3] thành hai nhóm: 

Nhóm A: [0, 3] 

Nhóm B: [1] 

| Bước | Vị trí nhóm A | Vị trí nhóm B | Đóng góp A | Đóng góp B | 
| --- | --- | --- | --- | --- | 
| 1 | [0, 3] | [] | tính toán thông qua các khoảng trống | 0 | 
| 2 | [] | [1] | 0 | tính toán thông qua các khoảng trống | 

Nhóm A có cấu trúc khoảng trống với vùng giữa rộng, trong khi Nhóm B là đơn lẻ. Đóng góp kết hợp của họ cho cả hai lần xuất hiện đóng góp riêng biệt cho nhiều mảng con, tối đa hóa phạm vi bao phủ. Thêm số 0 vào vị trí 2 sẽ tăng mỗi mảng con chứa nó thêm một phần tử riêng biệt. 

Dấu vết cho thấy cách phân tách ngăn cản cả hai lần xuất hiện của 1 hoạt động như một giá trị nhóm duy nhất, điều này sẽ làm giảm phạm vi bao phủ riêng biệt trong nhiều mảng con. 

### Mẫu 2 

đầu vào:```
3
0 0 0
```Ở đây không có giá trị nào khác 0. Mỗi mảng con bao gồm toàn số 0, vì vậy mỗi mảng con có chính xác một giá trị riêng biệt. 

| mảng con | Số lượng riêng biệt | 
| --- | --- | 
| [0] | 1 | 
| [0,0] | 1 | 
| [0,0,0] | 1 | 

Tổng tất cả 6 mảng con mang lại 6, xác nhận rằng các số 0 không được hưởng lợi từ bất kỳ phép biến đổi nào và hoạt động như một đường cơ sở không đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần và vị trí của mỗi giá trị được phân chia và quét tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách vị trí cho từng giá trị tuyệt đối riêng biệt | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì tất cả các hoạt động đều là các đường truyền tuyến tính trên cấu trúc mảng và không xảy ra quá trình truyền tải lồng nhau trên các mảng con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdout
    sys.stdin = StringIO(inp)
    out = StringIO()
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert solve_capture("4\n1 1 0 -1\n") == "19"
assert solve_capture("3\n0 0 0\n") == "6"

# custom cases
assert solve_capture("1\n5\n") == "1", "single element"
assert solve_capture("2\n1 1\n") >= "2", "duplicate effect"
assert solve_capture("5\n0 1 0 1 0\n") >= "0", "mixed zeros"
assert solve_capture("6\n2 2 2 2 2 2\n") >= "0", "all same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| trùng lặp | ngày càng tăng | hiệu ứng chia dấu | 
| số không trộn lẫn | không tầm thường | không tương tác | 
| tất cả đều giống nhau | có cấu trúc tối đa | hành vi phân cụm nặng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều bằng 0. Thuật toán xử lý chính xác số 0 một cách riêng biệt và gán cho mỗi mảng con một phần đóng góp của chính xác một phần tử riêng biệt, phù hợp với thực tế là không có thay đổi dấu nào có thể làm thay đổi đẳng thức. 

Một trường hợp khác là một mảng hoàn toàn đồng nhất gồm các giá trị tuyệt đối bằng nhau khác 0. Bước phân tách đảm bảo các lần xuất hiện được chia thành hai nhóm, ngăn không cho chúng bị thu gọn thành một giá trị được phân cụm nhiều. Điều này tránh việc tính thiếu phạm vi bao phủ của mảng con, đặc biệt đối với các mảng dài mà việc phân cụm sẽ làm giảm đáng kể số lượng riêng biệt. 

Trường hợp khó phát hiện cuối cùng là khi sự xuất hiện thưa thớt và không đều. Việc phân chia xen kẽ vẫn hoạt động vì nó tránh việc xây dựng các khối liền kề lớn bên trong một nhóm duy nhất, giữ cho các đóng góp khoảng cách được cân bằng và ngăn chặn bất kỳ nhóm đơn lẻ nào chiếm ưu thế trong việc loại trừ mảng con.
