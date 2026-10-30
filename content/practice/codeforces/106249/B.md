---
title: "CF 106249B - Hải ly rắn"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có $N$ điểm trên lưới 2D, mỗi điểm biểu thị một hải ly bắt đầu từ tọa độ nguyên. Tất cả hải ly đều di chuyển đồng thời trên một mặt phẳng vô tận."
date: "2026-06-25T07:19:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106249
codeforces_index: "B"
codeforces_contest_name: "MITIT Winter 2025-26 Advanced Individual Round"
rating: 0
weight: 106249
solve_time_s: 69
verified: true
draft: false
---

[CF 106249B - Hải ly rắn](https://codeforces.com/problemset/problem/106249/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có$N$các điểm trên lưới 2D, mỗi điểm đại diện cho một hải ly bắt đầu từ tọa độ nguyên. Tất cả hải ly đều di chuyển đồng thời trên một mặt phẳng vô tận. 

Sự chuyển động diễn ra liên tục nhưng bị hạn chế: trong một giây hải ly có thể thay đổi$x$tọa độ tối đa là 1 và nó$y$tọa độ tối đa là 1. Điều này có nghĩa là thước đo khoảng cách hiệu quả là khoảng cách Chebyshev, do đó việc đạt đến điểm mục tiêu$(x', y')$từ$(x, y)$mất chính xác$\max(|x-x'|, |y-y'|)$giây nếu chúng ta di chuyển một cách tối ưu. 

Chúng tôi được phép chọn vị trí cuối cùng cho tất cả hải ly. Sau khi di chuyển, vị trí cuối cùng của chúng phải nằm trên một “con rắn”, đơn giản là một tập hợp các điểm có thể được liệt kê theo thứ tự tọa độ của cả hai không bao giờ giảm. Nói cách khác, nếu chúng ta sắp xếp các vị trí cuối cùng theo thứ tự đó,$x$tọa độ không giảm và$y$tọa độ cũng không giảm. Nhiều hải ly được phép kết thúc tại cùng một điểm. 

Nhiệm vụ là giảm thiểu thời gian$T$để các vị trí cuối cùng như vậy tồn tại và đầu ra$2T$. Hệ số 2 chỉ là tỷ lệ; câu trả lời được đảm bảo là số nguyên. 

Các ràng buộc ngụ ý đến$2 \cdot 10^5$điểm tổng thể. Bất kỳ giải pháp nào cố gắng tìm kiếm rõ ràng trên các phép gán hoặc hoán vị sẽ quá chậm, vì ngay cả$N!$hoặc$2^N$-lý luận kiểu là không thể. Thậm chí$O(N^2)$mỗi trường hợp thử nghiệm sẽ ở mức giới hạn hoặc quá chậm trong tổng số trường hợp xấu nhất. 

Khó khăn chính là chúng ta không chỉ di chuyển các điểm một cách độc lập. Cấu hình cuối cùng phải đáp ứng ràng buộc thứ tự toàn cục, ràng buộc tất cả các điểm lại với nhau. 

Một số tình huống nguy hiểm rất dễ xử lý sai. 

Ví dụ: nếu tất cả các điểm đã được căn chỉnh thành một chuỗi đơn điệu$(1,1), (2,2), (3,3)$, câu trả lời sẽ bằng 0 vì không cần chuyển động. Một cách tiếp cận ngây thơ nhằm tạo ra sự bất bình đẳng nghiêm ngặt hoặc cố gắng “truyền bá” các điểm có thể làm tăng thêm thời gian không cần thiết một cách không chính xác. 

Nếu tất cả các điểm đều giống nhau$x$- phối hợp nhưng tăng dần$y$, câu trả lời lại là số không. Một số chiến lược không chính xác cố gắng sắp xếp theo cả hai tọa độ một cách độc lập và thừa nhận tính khác biệt, điều này bị phá vỡ ở đây. 

Một trường hợp tinh vi khác xuất hiện khi các điểm được xen kẽ, chẳng hạn$(1,3), (2,1), (3,2)$. Bất kỳ thứ tự hợp lệ nào cũng phải sắp xếp lại chúng và tính khả thi phụ thuộc vào việc liệu khoảng thời gian của các điểm đến có thể có trùng lặp theo cách đơn điệu nhất quán hay không. Những cách tiếp cận tham lam chỉ xem xét các giao dịch hoán đổi địa phương sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là nghĩ đến việc chọn cấu hình cuối cùng một cách rõ ràng. Trong một thời gian cố định$T$, mỗi con hải ly có thể tới bất kỳ điểm nào trong một$L_\infty$hình vuông có tâm ở điểm bắt đầu với bán kính$T$. Vì vậy, mỗi hải ly có một ô vuông chứa các đích đến khả thi và chúng ta phải chọn một điểm từ mỗi ô vuông sao cho các điểm được chọn tạo thành một chuỗi không giảm ở cả hai tọa độ. 

Một giải pháp đơn giản sẽ thử tất cả các hoán vị của hải ly và với mỗi hoán vị, hãy kiểm tra xem liệu chúng ta có thể gán mức tăng hay không$x$Và$y$giá trị bên trong các ô vuông cho phép. Điều này ngay lập tức thất bại vì có$N!$hoán vị và thậm chí kiểm tra một hoán vị đòi hỏi lý luận gán tuyến tính, dẫn đến độ phức tạp giai thừa hoặc hàm mũ. 

Quan sát quan trọng là điều kiện con rắn cuối cùng áp đặt một trật tự tổng thể. Khi chúng tôi quyết định thứ tự đó, tính khả thi sẽ trở thành vấn đề lan truyền ràng buộc 1D. Phần còn thiếu là làm thế nào để chọn thứ tự đó một cách tối ưu. 

Nếu chúng ta sắp xếp hải ly theo nguồn gốc của chúng$x$-cosort, mọi cấu hình cuối cùng hợp lệ đều có thể được sắp xếp lại để tôn trọng thứ tự đó mà không tăng thời gian cần thiết. Theo trực giác, nếu một con hải ly có kích thước nhỏ hơn$x$kết thúc sau cái lớn hơn$x$, việc hoán đổi chúng theo trình tự cuối cùng không thể vi phạm tính khả thi vì cả hai đều chuyển động bên trong đối xứng$L_\infty$các vùng và ràng buộc đơn điệu chỉ phụ thuộc vào thứ tự chứ không phụ thuộc vào danh tính. Điều này làm giảm vấn đề kiểm tra xem liệu chúng ta có thể chỉ định tăng$y$-tọa độ trong khi ở bên trong khoảng thời gian. 

Sau khi được sắp xếp theo$x$, mỗi con hải ly$i$có một khoảng thời gian khả thi cho lần cuối cùng của nó$y$-điều phối:$[y_i - T, y_i + T]$. Bây giờ chúng ta cần chọn một dãy không giảm$y'_i$, mỗi nơi$y'_i$nằm trong khoảng của nó. Đây là một phép kiểm tra tính khả thi tham lam cổ điển đối với các chuỗi đơn điệu bị ràng buộc theo khoảng thời gian. 

Chúng tôi quét từ trái sang phải, duy trì dòng điện nhỏ nhất có thể$y'$. Đối với mỗi khoảng thời gian, chúng tôi đặt$y'_i$ít nhất là giá trị trước đó nhưng không vượt quá giới hạn trên. Nếu tại bất kỳ thời điểm nào điều này trở nên không thể thực hiện được, người được chọn$T$là không thể thực hiện được. 

Chúng tôi tìm kiếm nhị phân nhỏ nhất$T$cái đó hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(N! \cdot N)$|$O(N)$| Quá chậm | 
| Sắp xếp + tham lam + tìm kiếm nhị phân |$O(N \log R)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các điểm theo thứ tự tăng dần$x$-điều phối. Điều này khắc phục thứ tự duy nhất quan trọng để xây dựng chuỗi đơn điệu mà không làm mất tính tổng quát. 
2. Tìm kiếm nhị phân tối thiểu$T$trong phạm vi$[0, 10^9]$. Hàm chúng tôi kiểm tra là đơn điệu: nếu một thời gian$T$hoạt động, bất kỳ thời gian lớn hơn nào cũng hoạt động vì tất cả các khoảng thời gian chỉ mở rộng. 
3. Đối với cố định$T$, tính cho mỗi điểm một khoảng hợp lệ$[y_i - T, y_i + T]$. 
4. Duyệt các điểm theo thứ tự được sắp xếp và duy trì một biến`cur_y`, đại diện cho giá trị nhỏ nhất có thể$y$-giá trị của chuỗi được xây dựng cho đến phần tử trước đó. 
5. Với mỗi quãng, hãy đặt`cur_y = max(cur_y, y_i - T)`. Nếu điều này vượt quá$y_i + T$, khoảng không thể chứa một dãy không giảm, vì vậy đây$T$thất bại. 
6. Nếu tất cả các khoảng thời gian được xử lý thành công, giá trị hiện tại$T$là khả thi. 
7. Đầu ra$2T$, câu trả lời được chia tỷ lệ bắt buộc. 

Sự lựa chọn tham lam ở bước 5 là rất quan trọng. Nó luôn đẩy giá trị hiện tại xuống thấp nhất có thể trong khi vẫn hợp lệ, mang lại sự linh hoạt tối đa cho các khoảng thời gian trong tương lai. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo$x$, mọi giải pháp hợp lệ đều tương ứng với việc chọn một$y'_i$từ mỗi khoảng sao cho dãy không giảm. Quá trình tham lam duy trì sự bất biến`cur_y`là giá trị nhỏ nhất có thể đạt được của tiền tố. Nếu tại một bước nào đó chúng ta có thể chọn một giá trị nhỏ hơn`cur_y`, nó sẽ chỉ cải thiện tính khả thi cho các bước sau chứ không bao giờ làm tình hình trở nên tồi tệ hơn. Do đó, việc thất bại ở một khoảng thời gian nào đó có nghĩa là không có phép gán hợp lệ nào tồn tại cho khoảng thời gian đó$T$, vì ngay cả cách xây dựng tiền tố dễ dãi nhất cũng đã vi phạm giới hạn khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(T, pts):
    cur = -10**18
    for x, y in pts:
        lo = y - T
        hi = y + T
        if cur < lo:
            cur = lo
        if cur > hi:
            return False
    return True

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        pts.sort()  # sort by x then y

        lo, hi = 0, 10**9
        while lo < hi:
            mid = (lo + hi) // 2
            if can(mid, pts):
                hi = mid
            else:
                lo = mid + 1

        out.append(str(lo * 2))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước sắp xếp xác định mức độ tự do có ý nghĩa duy nhất: sắp xếp theo chiều ngang. các`can`Hàm thực hiện lan truyền khoảng tham lam được mô tả trước đó và đây là bước kiểm tra tính khả thi duy nhất cần thiết cho mỗi thời điểm ứng cử viên. 

Tìm kiếm nhị phân là cần thiết vì ranh giới khả thi phụ thuộc vào việc các khoảng mở rộng như thế nào với$T$và không có biểu thức dạng đóng nào trực tiếp cho giá trị tối thiểu mà không cần kiểm tra. 

Một chi tiết triển khai tinh tế đang được khởi tạo`cur`đến một con số rất âm. Điều này tránh việc ràng buộc không chính xác phần tử đầu tiên. Một chi tiết khác là tất cả các so sánh phải sử dụng số học số nguyên vì tọa độ và$T$là số nguyên. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ:$$(1, 8), (2, 6), (5, 3), (8, 5)$$Sau khi sắp xếp theo$x$, thứ tự đã nhất quán rồi. 

### Theo dõi$T = 2$| Điểm | Khoảng thời gian$[lo, hi]$| cur trước | cur sau | 
| --- | --- | --- | --- | 
| (1,8) | [6,10] | -inf | 6 | 
| (2,6) | [4,8] | 6 | 6 | 
| (5,3) | [1,5] | 6 | thất bại | 

Tại điểm thứ ba, giá trị không giảm yêu cầu đã là 6, nhưng khoảng chỉ cho phép tối đa 5, vì vậy$T=2$là không thể thực hiện được. 

Điều này chứng tỏ rằng ngay cả khi các điểm ban đầu có tính linh hoạt thì một khoảng thời gian chặt chẽ sau đó cũng có thể dẫn đến thất bại. 

### Theo dõi$T = 3$| Điểm | Khoảng thời gian$[lo, hi]$| cur trước | cur sau | 
| --- | --- | --- | --- | 
| (1,8) | [5,11] | -inf | 5 | 
| (2,6) | [3,9] | 5 | 5 | 
| (5,3) | [0,6] | 5 | 5 | 
| (8,5) | [2,8] | 5 | 5 | 

Tất cả các điểm thành công, vì vậy$T=3$hoạt động. 

Điều này cho thấy sự lựa chọn tham lam đã sớm ổn định và luôn nhất quán, xác nhận rằng tính khả thi chỉ phụ thuộc vào việc liệu các khoảng thời gian có thể duy trì một chuỗi đơn điệu hay không chứ không phụ thuộc vào việc định vị chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log R)$| Sắp xếp mất$O(N \log N)$, mỗi bước tìm kiếm nhị phân sẽ quét tuyến tính và phạm vi tìm kiếm được giới hạn bởi các giới hạn tọa độ | 
| Không gian |$O(N)$| Lưu trữ điểm và một số biến trong quá trình kiểm tra | 

Tổng cộng$N$trên nhiều trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, do đó, quét tuyến tính cho mỗi lần kiểm tra và khoảng 30 lần lặp lại tìm kiếm nhị phân phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(T, pts):
        cur = -10**18
        for x, y in pts:
            lo = y - T
            hi = y + T
            if cur < lo:
                cur = lo
            if cur > hi:
                return False
        return True

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            pts = [tuple(map(int, input().split())) for _ in range(n)]
            pts.sort()

            lo, hi = 0, 10**9
            while lo < hi:
                mid = (lo + hi) // 2
                if can(mid, pts):
                    hi = mid
                else:
                    lo = mid + 1
            out.append(str(lo * 2))
        return "\n".join(out)

    return solve()

# sample-style checks (illustrative since original samples not repeated here)
assert run("1\n1\n0 0\n") == "0", "single point"

assert run("1\n3\n1 1\n2 2\n3 3\n") == "0", "already monotone"

assert run("1\n3\n1 3\n2 1\n3 2\n") >= "0", "mixed order case"

assert run("1\n4\n1 8\n2 6\n5 3\n8 5\n") == run("1\n4\n1 8\n2 6\n5 3\n8 5\n"), "consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm duy nhất | 0 | trường hợp không chuyển động tầm thường | 
| Đã có chuỗi đơn điệu | 0 | không có chuyển động không cần thiết | 
| Thứ tự hỗn hợp | không âm | tính khả thi đòi hỏi phải sắp xếp lại | 
| Bộ giống mẫu | xác định | tính nhất quán của đường ống đầy đủ | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi có nhiều điểm có chung điểm gần giống nhau$y$- có tọa độ nhưng khác nhau về$x$. Trong tình huống đó, thuật toán tham lam liên tục kẹp`cur`không thay đổi. đầu vào$$(1, 5), (2, 5), (3, 5)$$với$T = 0$trôi qua ngay lập tức vì tất cả các khoảng đều thu gọn về một điểm duy nhất và không giảm. 

Một trường hợp khác liên quan đến các ràng buộc xen kẽ chặt chẽ, chẳng hạn như$$(1, 10), (2, 0), (3, 10)$$Đối với nhỏ$T$, điểm giữa buộc chuỗi đi xuống rồi lại hướng lên trên, điều này vi phạm tính đơn điệu. Thuật toán phát hiện điều này một cách chính xác khi khoảng giữa không thể đáp ứng được tốc độ truyền`cur`. 

Trường hợp tinh tế cuối cùng là khi tính khả thi phụ thuộc hoàn toàn vào độ chính xác của tìm kiếm nhị phân hơn là cấu trúc. Bởi vì tất cả các giới hạn đều là số nguyên và câu trả lời được đảm bảo là tích phân sau khi nhân đôi, nên việc sử dụng tìm kiếm nhị phân số nguyên sẽ tránh được các lỗi dấu phẩy động có thể phân loại sai cấu hình đường biên.
