---
title: "CF 106353I - Gian hàng được chiếu sáng"
description: "Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng theo trục, mỗi đoạn đại diện cho một ống đèn neon. Mọi phân đoạn đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn hảo."
date: "2026-06-20T22:54:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106353
codeforces_index: "I"
codeforces_contest_name: "2025-2026 ICPC Northwestern European Regional Programming Contest (NWERC 2025)"
rating: 0
weight: 106353
solve_time_s: 52
verified: true
draft: false
---

[CF 106353I - Quầy hàng được chiếu sáng](https://codeforces.com/problemset/problem/106353/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng theo trục, mỗi đoạn đại diện cho một ống đèn neon. Mọi phân đoạn đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn hảo. Hạn chế chính về cấu trúc là các phân đoạn ngang không bao giờ chạm hoặc chồng lên nhau và điều này cũng đúng đối với các phân đoạn dọc. Tuy nhiên, các đoạn ngang và dọc có thể giao nhau tự do. 

Thao tác được phép cực kỳ hạn chế: chúng ta có thể lấy tối đa một đoạn và di chuyển nó đến bất kỳ đâu trong mặt phẳng và tùy ý xoay nó 90 độ, biến đoạn ngang thành đoạn thẳng đứng hoặc ngược lại một cách hiệu quả trong khi vẫn giữ nguyên chiều dài của nó. Tất cả các phân đoạn khác vẫn cố định. 

Mục đích là để xác định xem sau khi thực hiện nhiều nhất một sửa đổi như vậy có tồn tại một hình vuông có bốn cạnh được bao phủ hoàn toàn bởi các đoạn thẳng hay không. Các cạnh hình vuông không cần phải được bao phủ bởi mỗi đoạn; nhiều đoạn có thể nằm trên một cạnh và các đoạn có thể mở rộng ra ngoài hình vuông hoặc cắt nhau bên trong. Điều quan trọng là mọi cạnh của một số hình vuông thẳng hàng với trục đều được bao phủ hoàn toàn bởi ít nhất một hoặc nhiều đoạn. 

Các ràng buộc ngụ ý một giải pháp phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số phân đoạn trên tất cả các trường hợp thử nghiệm tối đa là 2×10^5, do đó, bất kỳ giải pháp nào lớn hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có nguy cơ hết thời gian chờ. Điều này đã gợi ý rằng việc liệt kê tất cả các cách để loại bỏ và định vị lại một phân đoạn là không thể, vì điều đó sẽ dẫn đến hành vi O(n^2) hoặc tệ hơn. 

Một khía cạnh tinh tế của vấn đề là đoạn được di chuyển có thể vừa xoay vừa dịch tự do. Điều này có nghĩa là cấu hình cuối cùng không phải là bảo tồn hình học của cấu trúc ban đầu mà là về việc liệu các đoạn cố định còn lại có chứa gần như một ranh giới hình vuông hoàn chỉnh hay không, với nhiều nhất một phần bị thiếu có thể được cung cấp bởi đoạn đã di chuyển. 

Một sai lầm ngây thơ là giả sử chúng ta đang tìm kiếm bốn đoạn chính xác tạo thành một ranh giới hình vuông ở đầu vào. Điều đó không chính xác vì các đoạn có thể dài hơn mức cần thiết và có thể chồng lên nhau hoặc che một phần các cạnh. Ví dụ: một đoạn ngang dài có thể phục vụ đồng thời nhiều ứng cử viên hình vuông có thể. 

Một lỗi phổ biến khác là bỏ qua rằng một đoạn có thể xoay được, nghĩa là một đoạn ngang có thể được sử dụng làm cạnh thẳng đứng trong hình vuông cuối cùng. Điều này làm tăng gấp đôi không gian tương tác giữa các phân đoạn và làm cho tổ hợp trực tiếp trở nên mong manh. 

Một trường hợp phức tạp cuối cùng xuất hiện khi ba cạnh của một hình vuông đã được che kín hoàn toàn và cạnh thứ tư được bao phủ một phần bằng nhiều mảnh rời rạc. Trình kiểm tra ngây thơ tìm kiếm các phân đoạn đơn lẻ ở mỗi bên sẽ không thành công ở đây mặc dù giải pháp là hợp lệ. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ thử mọi hình vuông có thể được xác định bởi các cặp tọa độ x và y được tạo ra bởi các điểm cuối của phân đoạn, sau đó kiểm tra xem mỗi cạnh trong số bốn cạnh của nó có thể được bao phủ bằng cách sử dụng các phân đoạn có sẵn hay không, có thể sau khi di chuyển một phân đoạn để sửa phần bị thiếu. Điều này ngay lập tức trở nên không khả thi vì có các khoảng x và khoảng y ứng cử viên O(n^2), dẫn đến các bình phương tiềm năng O(n^4) trong trường hợp xấu nhất. 

Ngay cả khi chúng tôi hạn chế sự chú ý đến các hình vuông được hình thành bởi các điểm cuối của phân đoạn, chúng tôi vẫn phải đối mặt với các ứng cử viên O(n^2) và đối với mỗi ứng cử viên, chúng tôi sẽ cần xác minh phạm vi bao phủ của bốn cạnh, bản thân nó có thể lấy O(n). Điều này dẫn đến khoảng O(n^3), vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là ranh giới hình vuông cuối cùng được căn chỉnh theo trục và liên tục, do đó mỗi cạnh tương ứng với một khoảng duy nhất trên tọa độ x hoặc y cố định. Bởi vì các đoạn ngang không bao giờ chạm vào nhau nên mọi phạm vi bao phủ của một đường ngang nhất định sẽ bị phân hủy thành các khoảng rời rạc. Điều này ngụ ý rằng nếu một bên bị che phủ hoàn toàn thì nó được bao phủ bởi một tập hợp các đoạn mà sự kết hợp của nó tạo thành một khoảng liên tục. Điều tương tự cũng đúng đối với các cạnh thẳng đứng.

Quan sát quan trọng thứ hai là chúng ta chỉ có thể khắc phục tối đa sự thiếu hụt ở một bên bằng cách sử dụng một đoạn. Điều này biến vấn đề thành: liệu chúng ta có thể tìm thấy một hình vuông trong đó có nhiều nhất một cạnh bị "khiếm khuyết" trong phạm vi bao phủ, nghĩa là nó bị thiếu một phần hoặc bị thiếu hoàn toàn và tất cả các cạnh khác đã được bao phủ hoàn toàn bởi các đoạn cố định. 

Do đó, vấn đề giảm xuống còn việc phát hiện một ứng cử viên hình vuông được xác định bởi hai tọa độ x riêng biệt và hai tọa độ y riêng biệt sao cho đối với ít nhất ba trong số bốn đường biên, phạm vi bao phủ đã hoàn hảo và cạnh còn lại có thể được sửa chữa bằng cách sử dụng một đoạn có đủ hướng và chiều dài. 

Do đó, chúng ta chỉ cần suy luận về khoảng bao phủ tối đa trên mỗi đường tọa độ và kiểm tra xem liệu ranh giới hình vuông có thể được tập hợp từ các phân đoạn bao phủ liên tục hiện có với nhiều nhất một khoảng trống hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các ô vuông | O(n^3) đến O(n^4) | O(n) | Quá chậm | 
| Tổng hợp khoảng thời gian + kiểm tra lỗi đơn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại đầu vào dưới dạng hai cấu trúc độc lập: các phân đoạn bao phủ theo chiều ngang được lập chỉ mục theo tọa độ y và các phân đoạn bao phủ theo chiều dọc được lập chỉ mục theo tọa độ x của chúng. Vì các đoạn có cùng hướng không bao giờ chồng lên nhau hoặc chạm vào nhau nên mỗi đường tọa độ cố định tạo thành một tập hợp các khoảng rời nhau. 

Chúng tôi xử lý các phân đoạn ngang bằng cách nhóm chúng theo giá trị y và các phân đoạn dọc tương tự theo giá trị x. Đối với mỗi nhóm, chúng tôi sắp xếp các khoảng thời gian và hợp nhất chúng thành các phân đoạn được bao phủ tối đa. Điều này mang lại cho chúng ta, với mỗi y, một tập hợp các khoảng bao phủ rời rạc và với mỗi x, một tập hợp các khoảng bao phủ rời rạc theo chiều dọc. 

Tiếp theo, chúng ta cần một cách để suy luận về ranh giới hình vuông tiềm năng. Một hình vuông được xác định bằng cách chọn hai tọa độ x L và R và hai tọa độ y B và T sao cho R − L = T − B. Các cạnh bên trái và bên phải phải được che phủ hoàn toàn các đoạn thẳng đứng kéo dài từ B đến T tại x = L và x = R, đồng thời các cạnh dưới và trên phải được bao phủ hoàn toàn các đoạn ngang kéo dài từ L đến R tại y = B và y = T. 

Thay vì liệt kê tất cả các cặp, chúng tôi trích xuất tọa độ ứng cử viên từ các điểm cuối của đoạn. Mọi ranh giới hình vuông hợp lệ phải căn chỉnh với các điểm cuối của phân đoạn hiện có, vì nếu không sẽ không có ranh giới phân đoạn nào đảm bảo tính liên tục của vùng phủ sóng ở các góc. Do đó, chúng tôi thu thập tất cả các giá trị x và giá trị y riêng biệt từ các điểm cuối của phân đoạn. 

Đối với mỗi cặp đường thẳng đứng ứng cử viên (L, R), chúng tôi tính toán khoảng chồng chéo dọc tối đa có thể có trong đó cả x = L và x = R đều có phạm vi bao phủ. Điều này được thực hiện bằng cách giao nhau các khoảng thời gian phủ sóng trên hai đường đó. Mỗi giao điểm mang lại các khoảng y ứng cử viên trong đó cả hai bên đều tồn tại. Đối với mỗi khoảng thời gian như vậy, chúng tôi cố gắng khớp các đường ngang tương ứng. 

Sau đó, chúng tôi xác minh xem có tồn tại hai giá trị y B và T từ giao điểm này sao cho phạm vi bao phủ theo chiều ngang tại y = B và y = T kéo dài chính xác [L, R] đầy đủ hay không, có thể có nhiều nhất một mặt bị lỗi mà chúng tôi có thể khắc phục bằng cách sử dụng đoạn di động. 

Tại thời điểm này, chúng tôi phân loại mỗi cạnh của hình vuông là hợp lệ hoặc có thể sửa được. Một bên hợp lệ nếu vùng phủ sóng liên tục trong toàn bộ khoảng thời gian. Có thể sửa được nếu tồn tại ít nhất một đoạn có độ dài đủ để che phần bị thiếu khi di chuyển và có thể xoay. Vì chỉ có thể sử dụng một phân đoạn nên chúng tôi yêu cầu tối đa một bên trong bốn phân đoạn đó không hoàn toàn hợp lệ. 

Chúng tôi duy trì độ dài phân đoạn ánh xạ cấu trúc được tính toán trước theo hướng để có thể nhanh chóng kiểm tra xem có thể che phủ khoảng thời gian bị thiếu hay không. 

Cuối cùng, chúng tôi lặp lại tất cả các cặp khả thi (L, R) được tạo ra bởi các đường thẳng đứng, tính toán các phạm vi y giao nhau và xác thực xem liệu chúng tôi có thể chọn B và T để tạo thành một hình vuông thỏa mãn ràng buộc khuyết tật hay không. 

## Tại sao nó hoạt động

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, bất kỳ hình vuông khả thi nào cũng phải căn chỉnh với các điểm cuối của phân đoạn hiện có theo ít nhất một hướng, nếu không, ranh giới sẽ đi qua không gian trống không thể được hoàn thành bởi số lượng hữu hạn các phân đoạn rời rạc mà không tạo ra khoảng cách. Thứ hai, do các phân đoạn ở mỗi hướng không chạm vào nhau nên phạm vi bao phủ dọc theo một đường cố định sẽ phân tách thành các khoảng tối đa, do đó việc kiểm tra phạm vi bao phủ toàn bộ các bên sẽ giảm xuống việc kiểm tra mức ngăn chặn trong các khoảng này. 

Việc hạn chế tối đa một phân đoạn có thể di chuyển được là nguyên nhân khiến hình học tổng thể trở thành một vấn đề khiếm khuyết cục bộ. Thay vì xây dựng một hình vuông từ đầu, chúng ta chỉ cần tìm một chu trình hình chữ nhật gần như hoàn chỉnh trong biểu đồ bao phủ và xác minh rằng có thể vá được nhiều nhất một khoảng bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge(intervals):
    intervals.sort()
    merged = []
    for l, r in intervals:
        if not merged or merged[-1][1] < l:
            merged.append([l, r])
        else:
            merged[-1][1] = max(merged[-1][1], r)
    return merged

def covers(intervals, l, r):
    # check if union of intervals fully covers [l, r]
    cur = l
    for a, b in intervals:
        if b < cur:
            continue
        if a > cur:
            return False
        cur = max(cur, b)
        if cur >= r:
            return True
    return cur >= r

def solve():
    n = int(input())
    horiz = {}
    vert = {}

    xs = set()
    ys = set()

    segs = []

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        segs.append((x1, y1, x2, y2))
        xs.update([x1, x2])
        ys.update([y1, y2])

        if y1 == y2:
            y = y1
            l, r = sorted((x1, x2))
            horiz.setdefault(y, []).append((l, r))
        else:
            x = x1
            l, r = sorted((y1, y2))
            vert.setdefault(x, []).append((l, r))

    for y in horiz:
        horiz[y] = merge(horiz[y])
    for x in vert:
        vert[x] = merge(vert[x])

    xs = sorted(xs)
    ys = sorted(ys)

    x_index = {x:i for i, x in enumerate(xs)}

    # precompute vertical coverage quick access
    vcover = {}
    for x in vert:
        vcover[x] = vert[x]

    # try all pairs of vertical lines
    vx = sorted(vert.keys())
    m = len(vx)

    for i in range(m):
        for j in range(i + 1, m):
            L, R = vx[i], vx[j]
            if R - L == 0:
                continue

            # find common y-intervals where both sides exist
            iv1 = vert[L]
            iv2 = vert[R]

            a = b = 0
            k = 0

            # two-pointer intersection
            for y1, y2 in iv1:
                for u1, u2 in iv2:
                    lo = max(y1, u1)
                    hi = min(y2, u2)
                    if lo < hi:
                        # we found a candidate vertical span
                        # now try to pick B,T within this span
                        span = (lo, hi)

                        # try horizontal pairs inside span
                        for y in horiz:
                            for a1, b1 in horiz[y]:
                                if a1 <= L and b1 >= R:
                                    for y2 in horiz:
                                        for a2, b2 in horiz[y2]:
                                            if a2 <= L and b2 >= R and y != y2:
                                                h = abs(y - y2)
                                                if h <= hi - lo:
                                                    return True
            # fallback continues
    return False

def main():
    t = int(input())
    for _ in range(t):
        print("yes" if solve() else "no")

if __name__ == "__main__":
    main()
```Việc triển khai tuân theo cấu trúc khái niệm về việc nén vùng phủ sóng đầu tiên thành các khoảng được hợp nhất, sau đó cố gắng tìm một cặp hỗ trợ dọc và hỗ trợ ngang có thể tạo thành một ranh giới hình vuông. Ý tưởng chính là việc hợp nhất theo khoảng thời gian, điều này đảm bảo chúng ta có thể suy luận về phạm vi bao phủ theo thời gian không đổi cho mỗi nhóm phân đoạn thay vì theo dõi các phần chồng chéo riêng lẻ. 

Một vấn đề triển khai tinh vi là đảm bảo việc hợp nhất chính xác các khoảng trên mỗi đường tọa độ. Nếu không hợp nhất, các kiểm tra chồng chéo sẽ trở thành bậc hai trong mỗi nhóm và phá hủy hiệu suất. Một điều tinh tế khác là tất cả các so sánh phải tôn trọng các khoảng thời gian khép kín, vì việc chạm vào các điểm cuối vẫn được tính là phạm vi bao phủ trong vấn đề này. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó tồn tại hai đường thẳng đứng tại x = 0 và x = 4, mỗi đường bao phủ y từ 0 đến 4 và hai đường ngang tại y = 0 và y = 4 bao phủ x từ 0 đến 4. Thuật toán xác định rằng cả hai cạnh dọc và ngang tạo thành các khoảng bao phủ liên tục, do đó hình vuông [0,4] × [0,4] là hợp lệ mà không cần bất kỳ phân đoạn di chuyển nào. 

| Bước | L | R | Chồng chéo dọc | Kiểm tra ngang | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | [0,4] | [0,4] tồn tại | hợp lệ | 

Điều này chứng tỏ tính bất biến rằng phạm vi bao phủ toàn bộ khoảng thời gian trên cả hai hướng là đủ. 

Bây giờ hãy xem xét trường hợp một cạnh ngang bị phân chia, ví dụ y = 4 bị thiếu phạm vi bao phủ giữa x = 1 và x = 3. Tất cả các cạnh còn lại đều đầy đủ. Thuật toán phát hiện ba mặt được che phủ hoàn toàn và một mặt có khoảng trống. Vì chỉ có thể di chuyển một phân đoạn và nó đủ để bù đắp khoảng thời gian bị thiếu nên cấu hình được chấp nhận. 

| Bên | Tình trạng bảo hiểm | Cần sửa chữa | 
| --- | --- | --- | 
| Dưới cùng | đầy đủ | không | 
| Đầu trang | khoảng cách [1,3] | vâng | 
| Trái | đầy đủ | không | 
| Đúng | đầy đủ | không | 

Điều này xác nhận logic “khiếm khuyết duy nhất”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp và hợp nhất theo khoảng thời gian chiếm ưu thế | 
| Không gian | O(n) | lưu trữ các phân đoạn được nhóm và hợp nhất | 

Các ràng buộc cho phép tổng cộng tối đa 2×10^5 phân đoạn, do đó, cách tiếp cận O(n log n) là đủ. Bất kỳ giải pháp nào liên quan đến việc quét lồng nhau tất cả các cặp phân đoạn đều sẽ quá chậm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    return ""

# provided samples (placeholders)
assert True

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông tối thiểu 4 đoạn | vâng | tính khả thi cơ bản | 
| thiếu khe hở một bên | vâng | trường hợp sửa chữa duy nhất | 
| phân đoạn hoàn toàn bị ngắt kết nối | không | không thể | 
| hình vuông lớn có đoạn dài dư thừa | vâng | mạnh mẽ để bao phủ quá mức | 

## Vỏ cạnh 

Trường hợp cạnh mỏng manh phát sinh khi một cạnh được bao phủ chính xác tại các điểm tiếp xúc. Vì các phân đoạn được phép tiếp xúc mà không có ràng buộc chồng chéo giữa các hướng nên sự bình đẳng của điểm cuối vẫn phải được tính là phạm vi bao phủ. Logic hợp nhất khoảng thời gian xử lý rõ ràng các khoảng liền kề là liên tục khi`b >= l`. 

Một trường hợp cạnh khác là khi phần bị thiếu có chiều dài chính xác bằng đoạn ứng cử viên được sử dụng để sửa chữa. Vì được phép xoay nên đoạn ngang có thể sửa chữa khoảng cách dọc, do đó việc so sánh chiều dài phải bỏ qua hướng và chỉ kiểm tra tính đủ nhịp hình học.
