---
title: "CF 102331E - Giành chiến thắng dễ dàng"
description: "Đối với mỗi cạnh được chèn, chúng tôi biết hai điểm cuối của nó, số lượng viên đá trên đó và giá trị dương biểu thị số tiền chúng tôi kiếm được nếu cạnh đó được đưa vào biểu đồ đã chọn của chúng tôi."
date: "2026-08-13T03:37:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "E"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 226
verified: true
draft: false
---

[CF 102331E - Giành chiến thắng dễ dàng](https://codeforces.com/problemset/problem/102331/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi cạnh được chèn, chúng tôi biết hai điểm cuối của nó, số lượng viên đá trên đó và giá trị dương biểu thị số tiền chúng tôi kiếm được nếu cạnh đó được đưa vào biểu đồ đã chọn của chúng tôi. Sau mỗi lần chèn, chúng tôi muốn tổng giá trị tối đa của một tập hợp con các cạnh có sẵn để tạo thành một biểu đồ tốt. 

Phần lý thuyết trò chơi là trở ngại đầu tiên. Giả sử chúng ta chọn một số cạnh tạo thành các chu trình rời rạc. Trong đồ thị vô hướng, tập hợp các cạnh như vậy chính xác là đồ thị con Euler: mọi đỉnh đều có bậc chẵn. Nim đang thua người chơi đầu tiên khi XOR của tất cả các cỡ cọc được chọn bằng 0. Do đó, LHiC thắng chính xác khi tồn tại một tập hợp con cạnh khác rỗng có độ đỉnh đều chẵn và có kích thước cọc XOR bằng 0. 

Điều kiện đó có thể được viết dưới dạng đại số tuyến tính trên trường có hai phần tử. Biểu diễn một cạnh ((u,v)) bằng một vectơ nhị phân có các cạnh ở vị trí (u) và (v). Thêm 60 tọa độ nữa, một tọa độ cho mỗi bit trong kích thước cọc của nó (a). Vectơ kết quả cho cạnh (i) là 

[ 
x_i = e_{u_i} \oplus e_{v_i} \oplus (a_i\text{ trong 60 tọa độ cuối cùng}). 
] 

Đối với một tập hợp con các cạnh, việc XOR các vectơ của chúng cho kết quả bằng 0 chính xác khi mỗi đỉnh xuất hiện với số lần chẵn và mọi bit của tổng XOR đều bằng 0. Do đó, một đồ thị tốt chính xác khi các vectơ của tất cả các cạnh được chọn của nó độc lập tuyến tính trên (\mathbb F_2). Sự giảm thiểu này là quan sát trung tâm đằng sau giải pháp. Công thức cơ sở tuyến tính tương tự cũng là công thức được sử dụng trong các lời giải đã được công bố của bài toán này. 

Vì vậy, bài toán ban đầu trở thành bài toán tập độc lập có trọng số cực đại trực tuyến cho vectơ nhị phân. Sau mỗi lần chèn, chúng ta cần tổng trọng lượng tối đa của một tập hợp con độc lập của tất cả các vectơ đã thấy cho đến nay. 

Có nhiều nhất (n+60\le124) tọa độ. Kích thước nhỏ này là lý do giải pháp cơ sở tuyến tính có thể thực hiện được. Tuy nhiên, số cạnh có thể là (200000), do đó việc xây dựng lại cơ sở từ tất cả các cạnh trước đó sau mỗi truy vấn sẽ quá tốn kém. Thuật toán bậc hai hoặc hàm mũ về số cạnh bị loại trừ hoàn toàn. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. Đầu tiên là một đồ thị không có chu trình. Ví dụ,```
3 2
1 2 0 5
2 3 0 7
```Đồ thị luôn là một khu rừng nên mọi cạnh có sẵn đều có thể được chọn. Đầu ra đúng là```
5
12
```Một giải pháp nhất quyết xây dựng một chu trình trước khi chấp nhận một cạnh sẽ loại bỏ các cạnh này một cách không chính xác. Định nghĩa của một biểu đồ tốt là ngăn chặn một tập hợp con tuần hoàn chiến thắng, do đó, một biểu đồ không theo chu kỳ tự động là tốt. 

Trường hợp thứ hai là một chu trình có cọc XOR bằng 0. Coi như```
3 3
1 2 0 1
2 3 0 2
3 1 0 10
```Sau cạnh thứ ba, việc chọn cả ba cạnh sẽ tạo ra một chu trình và Nim XOR là (0). Do đó đồ thị ba cạnh không tốt và đồ thị tốt nhất có trọng số (10+2=12). Giải pháp chỉ kiểm tra xem các cạnh được chọn có chứa chu trình hay không sẽ loại bỏ không chính xác mọi đồ thị tuần hoàn, trong khi giải pháp chỉ kiểm tra tính độc lập của đồ thị thông thường sẽ bỏ qua điều kiện Nim. 

Trường hợp thứ ba là các cạnh song song. Coi như```
2 2
1 2 0 5
1 2 0 9
```Hai vectơ cạnh bằng nhau nên chúng phụ thuộc tuyến tính. Đồ thị tốt nhất chỉ chứa cạnh thứ hai, cho```
5
9
```Việc triển khai bất cẩn coi mỗi cặp điểm cuối như một cạnh duy nhất có thể sẽ thất bại trong trường hợp này. 

Cuối cùng, giá trị cọc có thể chính xác là (2^{60}-1). Ví dụ,```
2 2
1 2 1152921504606846975 3
1 2 1152921504606846975 10
```Các vectơ bằng nhau nên đáp án là```
3
10
```Giá trị đá phải được biểu thị bằng tất cả 60 bit, bao gồm cả bit 59. Việc sử dụng biểu diễn 32 bit hoặc 64 bit có dấu mà không cẩn thận sẽ làm mất thông tin. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu từ việc mô tả đặc tính của một biểu đồ tốt. Đối với mỗi truy vấn, chúng ta có thể liệt kê mọi tập hợp con của các cạnh được thấy cho đến nay, kiểm tra xem các vectơ của nó có độc lập tuyến tính hay không và giữ tổng trọng số tối đa. Điều này đúng vì tính độc lập tuyến tính chính xác là định nghĩa của một đồ thị được chọn tốt sau phép rút gọn ở trên. Tuy nhiên, với (i) các cạnh có sẵn, có (2^i) tập hợp con và kiểm tra một tập hợp con bằng chi phí loại bỏ Gaussian (O(i(n+60))). Ở truy vấn cuối cùng, công việc trong trường hợp xấu nhất là 

[ 
\Theta(2^q q(n+60)), 
] 

điều này thậm chí chỉ có vài chục cạnh đã là vô vọng rồi chứ chưa nói đến (q=200000). 

Nếu tất cả các truy vấn đều được biết trước và chúng ta chỉ cần câu trả lời cho tập cuối cùng thì thuật toán tham lam matroid có trọng số tiêu chuẩn sẽ giải quyết được vấn đề. Sắp xếp tất cả các cạnh theo trọng số giảm dần và chèn một cạnh bất cứ khi nào vectơ của nó độc lập với các vectơ đã chọn. Tính độc lập tuyến tính tạo thành một matroid, do đó điều này tạo ra một tập độc lập có trọng số cực đại. Khó khăn là các truy vấn yêu cầu mọi tiền tố, trong khi các trọng số lại có thứ tự tùy ý. Chúng tôi không thể sắp xếp lại tiền tố hiện tại sau mỗi lần chèn. 

Quan sát quan trọng là kích thước rất nhỏ. Một tập hợp độc lập được chọn chứa tối đa (n+60\le124) cạnh, bất kể có bao nhiêu cạnh được chèn vào. Khi một vectơ mới phụ thuộc vào cơ sở hiện tại, nó sẽ tạo ra một mạch. Cạnh mới có thể cải thiện cơ sở tối ưu chỉ bằng cách thay thế cạnh được chọn có trọng số tối thiểu trong mạch đó. Đây là thuộc tính trao đổi cơ sở có trọng số đằng sau giải pháp trực tuyến. Giải pháp đã xuất bản lưu trữ, đối với mỗi vectơ cơ sở, các vectơ được chọn ban đầu được XOR để thu được nó, sau đó sử dụng biểu diễn của vectơ đến phụ thuộc để tìm chính xác cạnh rẻ nhất có thể trao đổi. 

Sự biểu diễn là điều làm cho phần trực tuyến có thể quản lý được. Giả sử các cạnh được chọn hiện tại được lập chỉ mục từ (0) đến (r-1). Bên cạnh mỗi vectơ cơ sở Gaussian, chúng ta lưu trữ một mặt nạ bit trên (r) các cạnh được chọn này. Mặt nạ cho chúng ta biết vectơ gốc XOR nào được chọn cho vectơ cơ sở hiện tại. Khi một vectơ mới giảm về 0, mặt nạ tích lũy của nó mô tả mạch duy nhất được hình thành với cơ sở hiện tại. Chúng ta tìm trọng số nhỏ nhất trong số các cạnh được chọn xuất hiện trong mặt nạ đó. Nếu cạnh mới nặng hơn, chúng ta thay thế cạnh đó và cập nhật mọi biểu diễn cơ sở chứa nó. Vì có tối đa 124 vectơ cơ sở nên mỗi truy vấn chỉ thực hiện (O(n+60)) thao tác như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(2^q q(n+60))) | (O(q)) | Quá chậm | 
| Tối ưu | (O(q(n+60))) | (O((n+60)^2)) bit | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cung cấp cho mỗi đỉnh đồ thị một tọa độ nhị phân và cung cấp cho mỗi bit của mỗi giá trị cọc một tọa độ bổ sung. Đối với một cạnh ((u,v,a,w)), hãy xây dựng mặt nạ bit nguyên 

[ 
x=(1\ll u);|;(1\ll v);|;(a\ll n). 
] 

(n) bit đầu tiên mã hóa tính chẵn lẻ của đỉnh. 60 bit cuối cùng mã hóa Nim XOR. 

1. Duy trì cơ sở XOR Gaussian được lập chỉ mục theo bit được đặt cao nhất. Đối với mỗi vị trí cơ sở, lưu trữ cả vectơ thực và mặt nạ biểu diễn. Vectơ là XOR của các vectơ cạnh được chọn ban đầu được mô tả bởi mặt nạ biểu diễn đó. 

Mặt nạ biểu diễn là cần thiết vì vectơ được lưu trữ trong cơ sở Gaussian thường không còn là một cạnh ban đầu nữa. Chúng ta cần biết cạnh gốc nào tham gia vào phần phụ thuộc khi cạnh mới được chèn vào. 

1. Cung cấp cho mỗi cạnh đã chọn một ô cố định từ (0) đến (r-1), trong đó (r) là thứ hạng hiện tại. Lưu trữ trọng lượng của nó trong`weights[slot]`. Mặt nạ đại diện sử dụng các khe này làm bit của nó. 
2. Khi có cạnh mới xuất hiện, hãy khởi tạo biểu diễn của nó bằng một bit tương ứng với một khe tạm thời mới. Sau đó thực hiện phép loại bỏ Gaussian thông thường từ tọa độ cao nhất đến tọa độ thấp nhất. Bất cứ khi nào vectơ hiện tại có bit trục hiện tại và trục đó đã tồn tại, hãy XOR vectơ cơ sở được lưu trữ vào vectơ hiện tại và cả XOR mặt nạ biểu diễn của nó vào biểu diễn hiện tại. 

Cả hai thao tác phải được thực hiện cùng nhau vì chúng mô tả cùng một tổ hợp tuyến tính. 

1. Nếu việc loại bỏ đạt đến một trục chưa sử dụng, cạnh mới sẽ độc lập. Thêm vectơ hiện tại làm vectơ cơ sở mới, gán cho nó một vị trí cạnh được chọn mới, thêm trọng số của nó vào câu trả lời và lưu trữ biểu diễn một bit tương ứng. 

Không cần trao đổi vì việc tăng thứ hạng luôn cải thiện mục tiêu và tất cả các trọng số đều dương. 

1. Nếu việc loại bỏ làm giảm vectơ mới về 0 thì cạnh mới sẽ phụ thuộc vào cơ sở được chọn hiện tại. Mặt nạ biểu diễn tích lũy bây giờ mô tả một mạch khác trống chứa cạnh mới và một số cạnh được chọn. 

Tìm cạnh được chọn có trọng số tối thiểu trong số các bit đã đặt của biểu diễn này. Gọi khe của nó`f`. 

1. Nếu cạnh mới có trọng lượng không lớn hơn`weights[f]`, loại bỏ nó. Thay thế`f`bởi cạnh mới sẽ không làm tăng câu trả lời và việc giữ cạnh hiện tại sẽ duy trì cơ sở trọng số tối đa. 
2. Nếu cạnh mới nặng hơn thì thay khe`f`theo cạnh mới và thêm`new_weight - weights[f]`để trả lời. 

Bản thân các vectơ cơ sở không cần thay đổi. Đại diện của họ cần phải thay đổi. Mặt nạ đại diện của cạnh mới chính xác là mặt nạ phụ thuộc được tìm thấy trong quá trình loại bỏ. Vì nó chứa cạnh cũ`f`, XOR mặt nạ này vào mọi biểu diễn cơ sở có chứa`f`loại bỏ cạnh cũ và thay thế nó bằng cạnh mới trong khi vẫn giữ nguyên vectơ biểu diễn. 

1. In tổng trọng lượng duy trì sau mỗi lần chèn. 

### Tại sao nó hoạt động 

Vectơ tăng cường của một cạnh ghi lại chính xác hai điều kiện quan trọng đối với một tập hợp con tuần hoàn chiến thắng: bậc chẵn ở mọi đỉnh đồ thị và Nim XOR bằng 0. Do đó, một tập con khác rỗng của các cạnh được chọn mà LHiC có thể thắng tồn tại chính xác khi các vectơ tương ứng phụ thuộc tuyến tính. Do đó, một đồ thị tốt chính xác là một tập hợp độc lập tuyến tính của các vectơ này. 

Đối với tiền tố hiện tại, các cạnh được chọn được duy trì bằng thuật toán là độc lập. Nếu một vectơ mới độc lập thì việc thêm nó sẽ tăng thứ hạng và luôn có lợi vì trọng số của nó là dương. Nếu nó phụ thuộc, biểu diễn của nó sẽ cho ra mạch duy nhất được tạo bằng cách thêm nó vào cơ sở hiện tại. Thuộc tính trao đổi matroid nói rằng việc thay thế bất kỳ cạnh có trọng số tối thiểu nào của mạch này bằng cạnh mới sẽ duy trì tính độc lập. Việc chọn loại có trọng lượng tối thiểu sẽ mang lại sự cải thiện lớn nhất có thể từ cạnh mới này. 

Các mặt nạ biểu diễn duy trì thông tin mạch này ngay cả khi việc loại bỏ Gaussian làm biến đổi các vectơ cơ sở được lưu trữ. Sau khi trao đổi, XOR mặt nạ phụ thuộc vào mọi biểu diễn bị ảnh hưởng sẽ thay đổi nhận dạng của cạnh được chọn trong khi vẫn giữ nguyên mọi vectơ được biểu thị. Do đó, cơ sở được lưu trữ tiếp tục mở rộng chính xác các vectơ cạnh đã chọn và tập hợp đã chọn vẫn là tập hợp độc lập có trọng số tối đa sau mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    D = n + 60

    # basis[p] is a vector whose highest set bit is p.
    # rep[p] tells which selected original edges XOR to basis[p].
    basis = [0] * D
    rep = [0] * D

    # weights[k] is the weight of the selected edge occupying slot k.
    weights = []

    rank = 0
    answer = 0

    def add_vector(v, w):
        nonlocal rank, answer

        # Temporary representation for the new edge.
        d = 1 << rank

        # Gaussian elimination.
        for p in range(D - 1, -1, -1):
            if not (v >> p) & 1:
                continue

            if basis[p] == 0:
                basis[p] = v
                rep[p] = d
                weights.append(w)
                rank += 1
                answer += w
                return

            v ^= basis[p]
            d ^= rep[p]

        # v == 0, so the new edge is dependent.
        # d describes the circuit in terms of selected edges.
        min_slot = -1
        min_weight = 10**30

        x = d
        while x:
            low = x & -x
            slot = low.bit_length() - 1

            if weights[slot] < min_weight:
                min_weight = weights[slot]
                min_slot = slot

            x ^= low

        # Keeping the old minimum-weight edge is at least as good.
        if w <= min_weight:
            return

        # Replace the minimum-weight edge by the new edge.
        answer += w - min_weight
        weights[min_slot] = w

        bit = 1 << min_slot

        # Every basis representation containing the old edge must
        # exchange it for the new edge. XOR with d performs exactly
        # that substitution.
        for p in range(D):
            if rep[p] & bit:
                rep[p] ^= d

    out = []

    for _ in range(q):
        u, v, a, w = map(int, input().split())
        u -= 1
        v -= 1

        vector = (1 << u) | (1 << v) | (a << n)

        add_vector(vector, w)
        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`sửa kích thước vectơ tại`n + 60`. Tọa độ đỉnh chiếm vị trí`0`bởi vì`n - 1`, trong khi các mũi cọc bắt đầu ở vị trí`n`. Dịch chuyển`a`qua`n`đặt tất cả 60 bit cọc vào khối tọa độ độc lập chính xác.`basis[p]`là vectơ cơ sở Gaussian có bit được đặt cao nhất là`p`. Đây là cách biểu diễn cơ sở XOR tiêu chuẩn, do đó, việc chèn tối đa cần`D`các bước loại bỏ. 

Phần tinh tế là`rep[p]`. Nó không phải là một giá trị số trong biểu đồ. Đó là một bitset có bit`k`có nghĩa là khe cạnh đã chọn`k`tham gia vào biểu thức XOR cho`basis[p]`. Bất cứ khi nào một vectơ cơ sở được XOR vào vectơ hiện tại, thì biểu diễn của nó cũng phải được XOR. 

Khi một vectơ mới độc lập,`d`là một bit đơn vì vectơ cơ sở mới chỉ được biểu thị bằng cạnh mới được chèn vào. Cạnh có một khe mới và câu trả lời sẽ tăng theo trọng số của nó. 

Khi`v`trở thành số không,`d`là mối quan hệ phụ thuộc. Vòng lặp sử dụng`low = x & -x`trích xuất các bit đã thiết lập của nó một cách hiệu quả mà không cần quét tất cả các chỉ số cạnh có thể có. Vì có tối đa 124 cạnh được chọn nên vòng lặp này rất nhỏ. 

Bước thay thế rất dễ xảy ra sai sót. Giả sử khe`f`là cạnh trọng lượng tối thiểu cũ và`d`là mặt nạ phụ thuộc của cạnh đến. Từ`f`thuộc về`d`, XOR`d`thành một biểu diễn cơ sở chứa`f`loại bỏ cạnh cũ và giới thiệu cạnh mới. Vectơ được biểu diễn không thay đổi, nhưng cạnh ban đầu được chọn liên quan đến biểu diễn đó thì có. 

Sự so sánh thật chặt chẽ`w <= min_weight`để từ chối. Với các trọng số bằng nhau, một trong hai cạnh cho cùng một giá trị khách quan, do đó việc giữ nguyên cơ sở hiện tại là đủ và tránh những thay đổi biểu diễn không cần thiết. 

Số nguyên Python có độ chính xác tùy ý, do đó cả vectơ đồ thị 124 bit và mặt nạ biểu diễn 124 bit đều được xử lý trực tiếp. Không xảy ra tràn trong cấu trúc vectơ hoặc trong câu trả lời, có mức tối đa lớn nhất là (200000\cdot10^9=2\cdot10^{14}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3
1 2 0 1
2 3 0 1
3 1 0 2
```Tất cả các giá trị cọc đều bằng 0, do đó tọa độ 60 bổ sung bằng 0. Do đó, vấn đề chỉ là lựa chọn rừng có trọng số tối đa. 

| Truy vấn | Cạnh | Cân nặng | Kết quả chèn | Xếp hạng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1-2 | 1 | Độc lập, thêm | 1 | 1 | 
| 2 | 2-3 | 1 | Độc lập, thêm | 2 | 2 | 
| 3 | 3-1 | 2 | Phụ thuộc, mạch chứa cạnh 1 và 2 | 2 | 3 | 

Ở truy vấn thứ ba, cạnh mới hoàn thành tam giác. Biểu diễn phụ thuộc của nó chứa hai cạnh được chọn đầu tiên. Trọng số của chúng đều là 1, do đó một trong số chúng được thay thế bằng cạnh mới có trọng số 2. Đồ thị được chọn vẫn có hai cạnh độc lập và hiện có tổng trọng số là 3. 

Do đó, kết quả đầu ra là```
1
2
3
```Ví dụ này chứng tỏ tại sao một cạnh phụ thuộc không thể bị loại bỏ một cách đơn giản. Một cạnh phụ thuộc nặng hơn có thể thay thế một cạnh nhẹ hơn trong mạch và cải thiện cơ sở tối ưu. 

### Mẫu 2 

Đầu vào là```
6 6
1 2 1 1
2 3 1 2
3 4 1 3
4 1 1 4
5 6 1 2
6 5 1 1
```Bốn cạnh đầu tiên tạo thành bốn chu kỳ và mỗi cạnh trong số chúng có giá trị cọc là 1. XOR của cả bốn giá trị cọc đều bằng 0, do đó bốn vectơ chu kỳ là phụ thuộc. 

| Truy vấn | Cạnh | Cân nặng | Kết quả chèn | Xếp hạng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1-2, a=1 | 1 | Độc lập, thêm | 1 | 1 | 
| 2 | 2-3, a=1 | 2 | Độc lập, thêm | 2 | 3 | 
| 3 | 3-4, a=1 | 3 | Độc lập, thêm | 3 | 6 | 
| 4 | 4-1, a=1 | 4 | Tùy thuộc, mạch chứa cả bốn | 3 | 9 | 
| 5 | 5-6, a=1 | 2 | Độc lập, thêm | 4 | 11 | 
| 6 | 6-5, a=1 | 1 | Phụ thuộc, cùng vectơ với cạnh 5 | 4 | 11 | 

Ở truy vấn 4, mạch chứa tất cả bốn cạnh. Trọng số tối thiểu là 1, do đó cạnh của trọng số 1 được thay thế bằng cạnh mới của trọng số 4. Tổng số tốt nhất trở thành (2+3+4=9). 

Cạnh thứ năm độc lập vì nó có đỉnh 5 và đỉnh 6 nên trọng số 2 của nó được thêm vào. Cạnh thứ sáu có vectơ tăng cường giống hệt cạnh thứ năm, nhưng trọng số của nó chỉ bằng 1 nên không thể cải thiện cơ sở. 

Các đầu ra là```
1
3
6
9
11
11
```Ví dụ này thực hiện cả hai loại phụ thuộc: một chu trình chính hãng có Nim XOR bằng 0 và hai cạnh song song mang cùng giá trị cọc. 

## Phân tích độ phức tạp 

hãy để 

[ 
D=n+60\le124. 
] 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(qD)) | Mỗi lần chèn thực hiện nhiều nhất (D) bước Gaussian, nhiều nhất (D) bước để tìm cạnh có trọng số tối thiểu trong phần phụ thuộc và nhiều nhất là cập nhật biểu diễn (D). | 
| Không gian | (O(D^2)) bit | Có nhiều nhất (D) vectơ cơ sở và mỗi biểu diễn là một số nguyên bit (D). | 

Thứ nguyên chỉ được giới hạn bởi 124, do đó thuật toán thực hiện một lượng công việc không đổi nhỏ cho mỗi truy vấn. Với (q\le200000), tổng số lần lặp tọa độ cơ sở tỷ lệ với khoảng (25) triệu, thay vì phụ thuộc vào số tập hợp con cạnh có thể có. Bản thân trạng thái chỉ chứa một số lượng không đổi các đối tượng 124 bit. 

Vấn đề ban đầu có giới hạn 1,5 giây và việc triển khai dự định sử dụng các bit 128 bit có kích thước cố định, đó là lý do tại sao cấu trúc (O(qD)) tương tự lại đặc biệt nhanh trong C++. Việc triển khai Python sử dụng các số nguyên có độ chính xác tùy ý cho cùng một hoạt động bitset và giữ cho tất cả trạng thái được giới hạn bởi cơ sở 124 chiều. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên đã được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng.```python
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """3 3
1 2 0 1
2 3 0 1
3 1 0 2
"""
) == "1\n2\n3", "sample 1"

# Sample 2
assert run(
    """6 6
1 2 1 1
2 3 1 2
3 4 1 3
4 1 1 4
5 6 1 2
6 5 1 1
"""
) == "1\n3\n6\n9\n11\n11", "sample 2"

# Sample 3
assert run(
    """5 5
1 2 0 1
2 3 1 1
3 4 2 3
4 5 4 9
5 1 7 29
"""
) == "1\n2\n5\n14\n42", "sample 3"

# Sample 4
assert run(
    """5 1
3 5 35 35
"""
) == "35", "sample 4"

# Minimum-size input.
assert run(
    """2 1
1 2 0 7
"""
) == "7", "minimum size"

# All equal vectors and equal weights.
assert run(
    """3 3
1 2 0 5
2 3 0 5
3 1 0 5
"""
) == "5\n10\n10", "equal values and equal weights"

# Parallel edges with different weights.
assert run(
    """2 3
1 2 0 4
1 2 0 9
1 2 0 6
"""
) == "4\n9\n9", "parallel edges"

# Maximum 60-bit pile value.
MAX_A = (1 << 60) - 1
assert run(
    f"""2 2
1 2 {MAX_A} 3
1 2 {MAX_A} 10
"""
) == "3\n10", "60-bit boundary"

# Maximum q and n, with all vectors identical.
# Every prefix has rank one, so the answer is always 1.
q = 200000
lines = [f"64 {q}"]
lines.extend(["1 2 0 1"] * q)
maximum_case = "\n".join(lines) + "\n"

out = run(maximum_case)
maximum_lines = out.splitlines()

assert len(maximum_lines) == q, "maximum q"
assert maximum_lines[0] == "1", "maximum q first answer"
assert maximum_lines[-1] == "1", "maximum q last answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, một cạnh |`7`| Số đỉnh và truy vấn tối thiểu, cộng với biểu đồ chu kỳ | 
| Tam giác ba cạnh có tất cả`a=0`và tất cả các trọng lượng`5`|`5, 10, 10`| Trọng lượng bằng nhau và điều kiện thay thế nghiêm ngặt | 
| Ba cạnh song song có trọng lượng`4,9,6`|`4, 9, 9`| Các vectơ bằng nhau và giữ đại diện tốt nhất | 
| Hai cạnh giống hệt nhau với (a=2^{60}-1) |`3, 10`| Bit cọc được phép cao nhất và xử lý ranh giới 64 bit | 
|`n=64`,`q=200000`, các cạnh giống nhau |`1`cho mọi tiền tố | Kích thước và số lượng truy vấn tối đa | 

## Vỏ cạnh 

Đồ thị không có chu trình được xử lý một cách tự nhiên vì một tập hợp các vectơ cạnh độc lập tương ứng với một đồ thị tốt bất kể nó có chứa chu trình hay không. Vì```
3 2
1 2 0 5
2 3 0 7
```vectơ đầu tiên tạo hạng 1 và đóng góp 5. Vectơ thứ hai tạo hạng 2 và đóng góp 7. Không gặp phải sự phụ thuộc nào, vì vậy đầu ra là`5`theo sau là`12`. 

Chu trình zero-Nim-XOR được xử lý bởi tọa độ cọc bổ sung. Vì```
3 3
1 2 0 1
2 3 0 2
3 1 0 10
```hai vectơ đầu tiên là độc lập. Số thứ ba giảm về 0 vì XOR của cả ba vectơ cạnh đều bằng 0. Phần phụ thuộc của nó chứa hai cạnh được chọn đầu tiên, có trọng số tối thiểu là 1. Vì cạnh đến có trọng số 10 nên cạnh đó sẽ thay thế cạnh có trọng số 1. Thứ hạng kết quả vẫn là 2 và câu trả lời là 12. Kết quả đầu ra là`1`,`3`,`12`. 

Các cạnh song song được xử lý như các vectơ thông thường thay vì theo cặp điểm cuối. Vì```
2 2
1 2 0 5
1 2 0 9
```cả hai vectơ tăng cường đều bằng nhau. Lần chèn thứ hai giảm về 0 và phần phụ thuộc của nó chứa cạnh đầu tiên. Vì 9 lớn hơn 5 nên cạnh được chọn đầu tiên sẽ được thay thế. Đầu ra là`5`,`9`. 

Trọng lượng bằng nhau không cần trao đổi. Vì```
2 3
1 2 0 5
1 2 0 5
1 2 0 5
```cạnh đầu tiên được chọn. Mỗi cạnh sau phụ thuộc vào nó, nhưng trọng số tối thiểu trong phụ thuộc cũng là 5. Vì trọng số mới không lớn hơn nên cơ sở không thay đổi và mọi câu trả lời vẫn là 5. 

Giá trị cọc lớn nhất sử dụng tất cả 60 tọa độ cọc có sẵn. Vì```
2 2
1 2 1152921504606846975 3
1 2 1152921504606846975 10
```cả hai vectơ đều giống hệt nhau, vì các bit điểm cuối của chúng và tất cả 60 bit cọc đều trùng nhau. Cạnh thứ hai thay thế cạnh thứ nhất vì trọng lượng của nó lớn hơn. Các đầu ra là`3`Và`10`. 

Cuối cùng, khi (q=200000), số cạnh được chèn lớn hơn nhiều so với thứ hạng tối đa có thể có. Đây chính xác là tình huống mà việc lưu trữ mọi cạnh trước đó sẽ không cần thiết đối với thuật toán cốt lõi. Sau tối đa 124 lần chèn độc lập thành công, thứ hạng sẽ không bao giờ tăng lên nữa. Các cạnh sau chỉ tạo các mạch phụ thuộc và có thể trao đổi các cạnh đã chọn. Thử nghiệm kích thước tối đa với các vectơ giống hệt nhau thực hiện hành vi này cho giới hạn truy vấn đầy đủ mà không tăng kích thước cơ sở vượt quá một.
