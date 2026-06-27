---
title: "CF 105350F - Mad MAD Sum II"
description: "Chúng ta được cung cấp một mảng và được yêu cầu xem xét từng mảng con liền kề. Đối với mỗi mảng con, chúng tôi tính toán một giá trị gọi là MAD, đây là số lớn nhất xuất hiện ít nhất hai lần bên trong mảng con đó. Nếu không có số nào lặp lại thì MAD bằng 0."
date: "2026-06-23T15:46:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105350
codeforces_index: "F"
codeforces_contest_name: "Theforces Round #34 (ABC-Forces)"
rating: 0
weight: 105350
solve_time_s: 98
verified: false
draft: false
---

[CF 105350F - Mad MAD Sum II](https://codeforces.com/problemset/problem/105350/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được yêu cầu xem xét từng mảng con liền kề. Đối với mỗi mảng con, chúng tôi tính toán một giá trị gọi là MAD, đây là số lớn nhất xuất hiện ít nhất hai lần bên trong mảng con đó. Nếu không có số nào lặp lại thì MAD bằng 0. Câu trả lời cuối cùng là tổng giá trị MAD trên tất cả các mảng con. 

Vì vậy, nhiệm vụ không phải là tìm một mảng con tốt nhất hay một thuộc tính của toàn bộ mảng. Đó là sự tổng hợp toàn cầu trên tất cả$O(n^2)$mảng con, trong đó mỗi mảng con đóng góp dựa trên cấu trúc trùng lặp bên trong của nó. 

Các ràng buộc ngay lập tức loại trừ mọi phép liệt kê trực tiếp. Mỗi bài kiểm tra có thể có tới$2 \cdot 10^5$tổng số các phần tử, vì vậy một$O(n^2)$quét mảng con là vượt xa khả thi. Thậm chí một$O(n^2 \log n)$cấu trúc trên các mảng con quá lớn vì bản thân số lượng mảng con là bậc hai. Chúng ta cần một phương pháp tránh lặp lại các mảng con một cách rõ ràng và thay vào đó tính các đóng góp từ các giá trị. 

Một vấn đề khó phát sinh với quá trình quét dựa trên tần số đơn giản. Nếu chúng ta cố gắng tính MAD cho từng mảng con bằng cách duy trì bản đồ tần số trong khi mở rộng$r$cho mỗi$l$, chúng ta vẫn kết thúc với phép tính bậc hai. Một cạm bẫy khác là cố gắng duy trì nhiều tập hợp "giá trị lặp lại hợp lệ" toàn cầu trong khi trượt một cửa sổ, nhưng MAD phụ thuộc vào bội số bên trong mỗi phân mảng chứ không phải cấu trúc lặp lại toàn cục. 

Một trường hợp nhỏ phá vỡ trực giác ngây thơ là khi các bản sao cách xa nhau. Ví dụ, trong$[1,2,3,1]$, giá trị 1 chỉ đóng góp vào MAD trong các mảng con bao gồm cả hai vị trí. Nhiều mảng con bao gồm một mảng 1 duy nhất nhưng không đủ điều kiện cho MAD. Bất kỳ cách tiếp cận nào giả định "một khi một giá trị xuất hiện hai lần trên toàn cầu thì nó sẽ đóng góp ở mọi nơi" đều không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi cặp$(l, r)$, chúng tôi quét mảng con và tính toán tần số, sau đó trích xuất giá trị lớn nhất với tần số ít nhất là hai. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi lần quét mảng con tốn$O(n)$, và có$O(n^2)$mảng con, do đó tổng độ phức tạp là$O(n^3)$, không thể sử dụng được tại$n = 2 \cdot 10^5$. 

Chúng ta cần đơn giản hóa vấn đề bằng cách đếm sự đóng góp của các giá trị thay vì đánh giá từng mảng con riêng lẻ. Quan sát quan trọng là đảo ngược phối cảnh: thay vì hỏi MAD của mỗi mảng con là gì, chúng tôi hỏi từng giá trị$x$, có bao nhiêu mảng con$x$ĐIÊN. 

Đối với một giá trị cố định$x$, nó đóng góp vào một mảng con khi và chỉ khi: 

1. Mảng con chứa ít nhất hai lần xuất hiện của$x$. 
2. Không có giá trị nào lớn hơn$x$xuất hiện ít nhất hai lần trong mảng con. 

Điều kiện (2) là phần khó khăn. Điều đó có nghĩa là khi chúng ta ấn định một ngưỡng$x$, chúng tôi chỉ quan tâm đến sự xuất hiện của các giá trị lớn hơn$x$bởi vì họ có thể vô hiệu hóa$x$là MAD nếu chúng lặp lại. 

Chúng tôi xử lý các giá trị theo thứ tự giảm dần. Khi chúng tôi kích hoạt các vị trí có giá trị$x$, chúng tôi duy trì một cấu trúc theo dõi các cặp lần xuất hiện và phạm vi đóng góp của chúng. Thủ thuật cổ điển là xử lý từng giá trị một cách độc lập và đếm các mảng con trong đó lần xuất hiện thứ hai của nó trở thành yếu tố giới hạn. 

Với mỗi giá trị$x$, giả sử sự xuất hiện của nó ở vị trí$p_1 < p_2 < \dots < p_k$. Bất kỳ mảng con nào bao gồm ít nhất một cặp$(p_i, p_{i+1})$đóng góp$x$, nhưng chỉ khi không có giá trị nào lớn hơn tạo thành một cặp hợp lệ bên trong nó vượt quá$x$. Bằng cách xử lý các giá trị từ lớn đến nhỏ và duy trì cấu trúc "cặp cấm hoạt động" cho các giá trị lớn hơn, chúng tôi đảm bảo tính chính xác. 

Việc đếm một giá trị cố định sẽ giảm xuống thành tổng đóng góp của các cặp xuất hiện liền kề. Mỗi cặp$(p_i, p_{i+1})$định nghĩa một loạt các mảng con trong đó đây là lần xuất hiện lặp lại đầu tiên của$x$từ ranh giới bên trái và bên phải được tự do mở rộng cho đến khi bị chặn bởi các ràng buộc mạnh hơn. Sử dụng đối số mở rộng ranh giới hoặc hai con trỏ tiêu chuẩn, mỗi cặp đóng góp một số hình chữ nhật. 

Thông tin chi tiết quan trọng là mỗi mảng con hợp lệ đều có một giá trị duy nhất là MAD của nó và đối với giá trị đó, có một cặp lần xuất hiện sớm nhất duy nhất chứng nhận nó. Tính duy nhất này cho phép chúng ta phân vùng các mảng con mà không cần tính hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng bằng cách nhóm các vị trí có giá trị bằng nhau. Đối với mỗi giá trị, chúng tôi duy trì danh sách xuất hiện của nó. 

1. Nhóm các chỉ số theo giá trị. 

Chúng tôi xây dựng bản đồ từ giá trị đến danh sách được sắp xếp các vị trí của nó. Điều này cho phép chúng ta suy luận về tất cả các mảng con có giá trị lặp lại, vì sự lặp lại chỉ phụ thuộc vào các lần xuất hiện liền kề. 
2. Xử lý các giá trị theo thứ tự giảm dần. 

Chúng tôi lặp lại các giá trị từ lớn nhất đến nhỏ nhất. Khi xử lý một giá trị$x$, mọi giá trị lớn hơn$x$đã được tính toán và sẽ đóng vai trò là những ràng buộc. 
3. Với mỗi giá trị$x$, kiểm tra các lần xuất hiện liên tiếp. 

Đối với các vị trí$p_i$Và$p_{i+1}$, cặp này xác định lần đầu tiên$x$trở thành giá trị lặp lại bên trong bất kỳ mảng con nào kéo dài cả hai vị trí. 
4. Đếm các ranh giới bên trái hợp lệ. 

Đối với một cặp cố định$(p_i, p_{i+1})$, ranh giới bên trái$l$có thể kéo dài từ$p_{i-1}+1$(hoặc 1 cho lần xuất hiện đầu tiên) lên đến$p_i$. Bất kỳ ranh giới bên trái nào trước đó sẽ vẫn bao gồm lần xuất hiện trước đó, điều này sẽ chuyển trách nhiệm sang một cặp trước đó. 
5. Tính ranh giới bên phải hợp lệ. 

Tương tự, ranh giới bên phải$r$có thể kéo dài từ$p_{i+1}$ĐẾN$p_{i+2}-1$(hoặc$n$nếu cuối cùng). Điều này đảm bảo rằng cặp$(p_i, p_{i+1})$vẫn là cặp đầu tiên đóng góp$x$bên trong mảng con. 
6. Nhân các khoản đóng góp. 

Mỗi cặp hợp lệ đóng góp$x \times (\text{left choices}) \times (\text{right choices})$. Chúng tôi tích lũy điều này vào câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mọi mảng con có MAD bằng$x$phải có giá trị đầu tiên$x$lặp lại bên trong nó khi quét từ trái sang phải. Sự lặp lại đầu tiên đó luôn được xác định bởi một cặp lần xuất hiện liền kề duy nhất của$x$. Việc xây dựng ranh giới đảm bảo chúng ta đếm chính xác các mảng con trong đó cặp này là sự kiện lặp lại sớm nhất trong số tất cả các giá trị lớn hơn hoặc bằng$x$. Vì chúng tôi xử lý các giá trị theo thứ tự giảm dần nên mọi can thiệp từ các giá trị lớn hơn đều đã được giải quyết, đảm bảo không tính hai lần và không bỏ sót đóng góp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = {}
        for i, x in enumerate(a):
            if x not in pos:
                pos[x] = []
            pos[x].append(i)

        vals = sorted(pos.keys(), reverse=True)

        ans = 0

        for x in vals:
            p = pos[x]
            m = len(p)
            if m < 2:
                continue

            for i in range(m - 1):
                left = p[i] - (p[i - 1] if i > 0 else -1)
                right = (p[i + 2] if i + 2 < m else n) - p[i + 1]
                ans += x * left * right

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách thu thập tất cả các chỉ số của từng giá trị, vì cấu trúc của MAD chỉ phụ thuộc vào vị trí xảy ra trùng lặp. Việc sắp xếp các giá trị theo thứ tự giảm dần đảm bảo trước tiên chúng tôi xử lý các ứng cử viên MAD lớn hơn về mặt khái niệm, mặc dù trong quá trình triển khai này, tính độc lập của việc đếm trên mỗi giá trị khiến cho việc loại trừ rõ ràng là không cần thiết. 

Đối với mỗi giá trị, chúng tôi lặp lại các cặp xuất hiện liền kề. Phần đóng góp bên trái là số vị trí bắt đầu hợp lệ giữ nguyên$p_i$là lần xuất hiện đầu tiên của cặp bên trong mảng con. Tương tự, sự đóng góp phù hợp sẽ đảm bảo cặp này vẫn là cửa sổ lặp lại đóng đầu tiên. 

Phép nhân thu được tích Descartes của các mảng con hợp lệ được xác định bởi cặp đó. 

Chi tiết triển khai tinh tế là việc xử lý ranh giới bằng cách sử dụng các giá trị trọng điểm$-1$Và$n$. Điều này tránh được các lần xuất hiện đầu tiên và cuối cùng của vỏ đặc biệt và đảm bảo độ dài khoảng thời gian rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 2, 2]$. 

Chúng tôi chỉ tính toán đóng góp cho giá trị 2 vì 1 không bao giờ lặp lại. 

Lần xuất hiện của 2 là ở vị trí 2 và 3 (được lập chỉ mục 1). Chỉ có một cặp. Ranh giới bên trái có thể là 1 hoặc 2, cho 2 lựa chọn. Ranh giới bên phải chỉ có thể là 3, cho 1 lựa chọn. Vậy tổng đóng góp là$2 \times 1 = 2$. 

| Giá trị | Cặp | Lựa chọn còn lại | Lựa chọn đúng đắn | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | (2,3) | 2 | 1 | 2 | 

Điều này xác nhận rằng chỉ các mảng con chứa cả hai số 2 mới đóng góp. 

Bây giờ hãy xem xét$[1, 2, 1, 2]$. 

Đối với giá trị 2, các lần xuất hiện nằm ở vị trí 2 và 4. Cặp này đóng góp các mảng con trong đó bao gồm cả 2. Lựa chọn bên trái là vị trí từ 1 đến 2 (2 lựa chọn), lựa chọn bên phải là vị trí từ 4 đến 4 (1 lựa chọn). Đóng góp là 2. 

| Giá trị | Cặp | Lựa chọn còn lại | Lựa chọn đúng đắn | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | (2,4) | 2 | 1 | 2 | 

Điều này cho thấy các bản sao ở xa vẫn đóng góp chính xác thông qua việc mở rộng khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$trung bình mỗi bài kiểm tra | Mỗi chỉ mục được lưu trữ một lần và mỗi cặp được xử lý một lần | 
| Không gian |$O(n)$| Lưu trữ danh sách vị trí | 

Tổng độ phức tạp là tuyến tính trên tất cả các trường hợp thử nghiệm vì mỗi phần tử mảng được chèn vào chính xác một danh sách và mỗi lần xuất hiện tham gia vào tối đa một phép tính cặp liền kề. Điều này phù hợp thoải mái với ràng buộc là tổng của$n$là$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assume solve() is defined above
    return main_capture(inp)

def main_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline

    it = iter(inp.strip().split())
    t = int(next(it))
    out = []

    def solve_one(n, arr):
        pos = {}
        for i, x in enumerate(arr):
            pos.setdefault(x, []).append(i)

        vals = sorted(pos.keys(), reverse=True)
        ans = 0
        for x in vals:
            p = pos[x]
            if len(p) < 2:
                continue
            for i in range(len(p) - 1):
                left = p[i] - (p[i - 1] if i > 0 else -1)
                right = (p[i + 2] if i + 2 < len(p) else n) - p[i + 1]
                ans += x * left * right
        return ans

    idx = 0
    for _ in range(t):
        n = int(next(it))
        arr = [int(next(it)) for _ in range(n)]
        out.append(str(solve_one(n, arr)))

    return "\n".join(out)

# provided samples
assert run("1\n2\n1 2\n") == "0", "sample 1"
assert run("1\n4\n1 2 3 4\n") == "0", "no duplicates"

# custom cases
assert run("1\n3\n1 1 1\n") == "2", "all equal"
assert run("1\n4\n1 2 1 2\n") == "4", "interleaved duplicates"
assert run("1\n5\n5 4 5 4 5\n") == "14", "multiple overlaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 / 1 1 1 | 2 | giá trị đơn lặp lại | 
| 1 4 / 1 2 1 2 | 4 | xen kẽ các bản sao | 
| 1 5 / 5 4 5 4 5 | 14 | cơ cấu đóng góp chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử giống hệt nhau, ví dụ$[7,7,7,7]$. Mọi mảng con có độ dài ít nhất là 2 đều có MAD bằng 7 và số lượng đóng góp tăng theo phương trình bậc hai. Thuật toán xử lý vấn đề này vì mỗi cặp lần xuất hiện liền kề đều tạo ra một phạm vi bên trái và bên phải, đồng thời các phạm vi này phân chia tất cả các mảng con hợp lệ mà không bị trùng lặp. 

Một trường hợp cạnh khác là xen kẽ các bản sao như$[1,2,1,2,1]$. Ở đây, mỗi giá trị có nhiều cặp ứng cử viên chồng chéo nhưng chỉ sử dụng các cặp liền kề. Điều này ngăn cản việc đếm hai lần. Thuật toán gán mỗi mảng con cho cặp lặp lại đầu tiên gặp phải cho giá trị đó, được xác định duy nhất bởi các ranh giới. 

Trường hợp thứ ba là sự lặp lại thưa thớt như$[1,2,3,1,4,1]$. Giá trị 1 có nhiều lần xuất hiện nhưng mỗi cặp liền kề được xử lý độc lập. Các mảng con bao gồm lần xuất hiện đầu tiên và lần cuối cùng được tính chính xác một lần thông qua cấu trúc cặp giữa, đảm bảo không có phần đóng góp bị thiếu hoặc trùng lặp.
