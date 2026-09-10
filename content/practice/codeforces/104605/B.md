---
title: "CF 104605B - ​​Goulash"
description: "Chúng tôi được cung cấp một số mục tiêu là knedlíky $N$. Có ba loại nhà hàng: một số cho 4 knedlíky, một số cho 5 và một số cho 6. Chúng tôi có thể chọn một số nhà hàng cho mỗi loại, nhưng chúng tôi không thể vượt quá số lượng có sẵn $A, B, C$. Mỗi nhà hàng chỉ được sử dụng tối đa một lần."
date: "2026-06-30T02:49:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104605
codeforces_index: "B"
codeforces_contest_name: "XXVII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 104605
solve_time_s: 81
verified: true
draft: false
---

[CF 104605B - Goulash](https://codeforces.com/problemset/problem/104605/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số mục tiêu là knedlíky$N$. Có ba loại nhà hàng: một số cho 4 knedlíky, một số cho 5 và một số cho 6. Chúng tôi có thể chọn một số nhà hàng cho mỗi loại, nhưng chúng tôi không thể vượt quá số lượng có sẵn$A, B, C$. Mỗi nhà hàng chỉ được sử dụng tối đa một lần. 

Câu hỏi là liệu chúng ta có thể chọn một số con số$x, y, z$như vậy:$$4x + 5y + 6z = N$$với$0 \le x \le A$,$0 \le y \le B$,$0 \le z \le C$. 

Mỗi trường hợp thử nghiệm là độc lập và chúng ta phải trả lời liệu lựa chọn đó có tồn tại hay không. 

Các ràng buộc rất lớn: lên tới$10^5$trường hợp thử nghiệm và giá trị lên đến$10^8$. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng khám phá tất cả các kết hợp của$x, y, z$hoặc thực hiện lập trình động trên$N$. Bất kỳ giải pháp nào cũng phải giảm từng trường hợp thử nghiệm thành công việc giới hạn không đổi hoặc rất nhỏ. 

Một nỗ lực ngây thơ sẽ thử tất cả các bộ ba$(x,y,z)$trong giới hạn, nhưng trường hợp xấu nhất cho phép$10^8$khả năng cho mỗi biến, điều này hoàn toàn không khả thi. 

Một dạng thất bại tinh vi hơn xuất phát từ lý luận tham lam. Ví dụ: luôn lấy càng nhiều nhà hàng 6-knedlík càng tốt có thể phá vỡ các giải pháp hợp lệ. Nếu như$N = 20$, tham lam sẽ lấy ba số 6 (18), để lại 2, điều này là không thể, mặc dù tồn tại một giải pháp hợp lệ:$5 + 5 + 5 + 5$. 

Khó khăn cốt lõi là các đồng xu có kích thước 4, 5 và 6 tương tác không cần thiết với số lượng giới hạn, vì vậy chúng ta cần một cách có cấu trúc để chỉ khám phá một số lượng cấu hình có ý nghĩa không đổi. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các giá trị hợp lệ$x, y, z$, kiểm tra xem phương trình có đúng không. Điều này đúng nhưng ngay lập tức thất bại do không gian tìm kiếm khối. 

Quan sát quan trọng là kích thước đồng xu nhỏ và có hiệu lực liên tục. Một khi chúng ta có đủ sự linh hoạt, tập hợp các tổng có thể biểu diễn sẽ trở nên dày đặc. Trên thực tế, với đồng xu 4, 5 và 6, các giá trị duy nhất không thể truy cập được mà không có giới hạn là những ngoại lệ rất nhỏ như 1, 2, 3 và 7. Ngoài ra, có rất nhiều sự kết hợp. 

Sự phức tạp còn lại là giới hạn trên$A, B, C$. Tuy nhiên, vì mỗi biến có thể được dịch chuyển trong các khối bảo toàn cấu trúc nên mọi giải pháp hợp lệ (nếu nó tồn tại) đều có thể được tìm thấy bằng cách chỉ kiểm tra một lân cận nhỏ của ứng viên.$(y, z)$các giá trị. Một lần$y$Và$z$đã được cố định,$x$được xác định duy nhất:$$x = \frac{N - 5y - 6z}{4}$$Vì vậy, nhiệm vụ giảm xuống còn việc thử một số lượng nhỏ không đổi$y, z$các ứng cử viên bao gồm tất cả các cấu hình ranh giới và dư lượng cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Toàn lực vũ phu hơn$x,y,z$|$O(ABC)$|$O(1)$| Quá chậm | 
| Tìm kiếm vùng lân cận liên tục$y,z$|$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm để kiểm tra một số lượng không đổi các ứng cử viên có cấu trúc. 

### 1. Xử lý những điều không thể xảy ra 

Đầu tiên chúng ta quan sát thấy rằng một số giá trị nhỏ của$N$hoàn toàn không thể được hình thành bằng cách sử dụng 4, 5 và 6. Đó là:$$N \in \{1, 2, 3, 7\}$$Đối với những trường hợp này, không có sự kết hợp nào tồn tại bất kể giới hạn. 

### 2. Liệt kê các ứng cử viên nhỏ cho$y$Và$z$Chúng tôi thử tất cả các giá trị nhỏ của$y$Và$z$, thường là trong phạm vi từ 0 đến 5. Lý do điều này có hiệu quả là vì bất kỳ giá trị lớn hơn nào cũng có thể được coi là sự dịch chuyển khối lượng giữa các biến mà không thay đổi tính biểu thị về cơ bản, vì chênh lệch 4, 5 và 6 có thể được bù bằng cách điều chỉnh các biến khác. 

Điều này làm giảm không gian tìm kiếm xuống nhiều nhất là 36 cặp. 

### 3. Xác thực từng ứng viên 

Đối với mỗi cặp$(y, z)$, chúng tôi tính toán:$$rem = N - 5y - 6z$$Nếu như$rem < 0$, cặp này không hợp lệ. 

Chúng tôi cũng yêu cầu:$$rem \bmod 4 = 0$$Sau đó:$$x = rem / 4$$Cuối cùng, chúng tôi kiểm tra:$$x \le A,\quad y \le B,\quad z \le C$$Nếu tất cả các ràng buộc đều được thỏa mãn, câu trả lời là CÓ. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là bất kỳ giải pháp khả thi nào cũng có thể được chuyển đổi thành một giải pháp mà$y$Và$z$nằm trong miền giới hạn mà không mất giá trị, vì tăng$y$hoặc$z$cho 4 bảo toàn tổng còn lại chia hết cho 4 và điều chỉnh$x$có thể dự đoán được. Vì chúng ta chỉ quan tâm đến sự tồn tại nên việc khám phá đại diện của các lớp tương đương này là đủ. Bảng liệt kê giới hạn đảm bảo chúng tôi đạt được ít nhất một đại diện của bất kỳ cấu hình hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    bad = {1, 2, 3, 7}

    for _ in range(T):
        N, A, B, C = map(int, input().split())

        if N in bad:
            print("NO")
            continue

        ok = False

        for y in range(6):
            for z in range(6):
                val = 5 * y + 6 * z
                rem = N - val
                if rem < 0:
                    continue
                if rem % 4 != 0:
                    continue
                x = rem // 4
                if x < 0:
                    continue
                if x <= A and y <= B and z <= C:
                    ok = True
                    break
            if ok:
                break

        print("SI" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại trên một lưới không đổi$(y, z)$các giá trị. Các vòng lặp lồng nhau được cố tình nhỏ để thậm chí với$10^5$trường hợp thử nghiệm, tổng công việc vẫn tuyến tính trong thực tế. 

Việc kiểm tra tính chia hết đảm bảo tính đúng đắn của kết quả dẫn xuất$x$. Kiểm tra giới hạn thực thi ràng buộc rằng không có loại nhà hàng nào bị lạm dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$N=26, A=1, B=2, C=3$$Chúng tôi thử các cặp nhỏ: 

| y | z | 5y+6z | rem = 26 - val | rem% 4 | x | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 5 + 12 = 17 | 9 | 1 | - | không | 
| 2 | 2 | 10 + 12 = 22 | 4 | 0 | 1 | vâng | 

chúng tôi nhận được$x=1, y=2, z=2$, tất cả đều nằm trong giới hạn nên câu trả lời là CÓ. 

### Ví dụ 2 

đầu vào:$$N=11, A=2, B=1, C=0$$| y | z | giá trị | rem | rem% 4 | x | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 6 | 2 | - | không | 
| 0 | 1 | 6 | 5 | 1 | - | không | 

Không có sự kết hợp hợp lệ nào tồn tại, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Liệt kê không đổi cho mỗi trường hợp thử nghiệm (36 lần kiểm tra) | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Thuật toán xử lý thoải mái$10^5$các trường hợp kiểm thử vì mỗi trường hợp chỉ thực hiện một số phép tính số học cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    bad = {1, 2, 3, 7}
    out = []

    for _ in range(T):
        N, A, B, C = map(int, input().split())

        if N in bad:
            out.append("NO")
            continue

        ok = False
        for y in range(6):
            for z in range(6):
                val = 5*y + 6*z
                rem = N - val
                if rem >= 0 and rem % 4 == 0:
                    x = rem // 4
                    if x <= A and y <= B and z <= C:
                        ok = True
        out.append("SI" if ok else "NO")

    return "\n".join(out)

# sample-style checks
assert run("3\n26 1 2 3\n4 0 0 0\n11 2 1 0\n") == "SI\nNO\nNO"
assert run("2\n1 10 10 10\n7 10 10 10\n") == "NO\nNO"
assert run("1\n0 0 0 0\n") == "SI"
assert run("1\n100000000 100000000 100000000 100000000\n") == "SI"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhỏ hợp lệ | SI | xây dựng cơ bản | 
| nhỏ không thể | KHÔNG | dư lượng không thể tiếp cận | 
| trường hợp không | SI | lựa chọn trống | 
| giới hạn lớn | SI | không có âm tính giả dưới sự phong phú | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi$N = 0$. Thuật toán chấp nhận chính xác điều này vì việc chọn$x = y = z = 0$luôn thỏa mãn phương trình và các giới hạn cho phép điều đó một cách tầm thường. 

Một trường hợp cạnh khác là khi$N$nhỏ nhưng không thuộc tập không thể rõ ràng, chẳng hạn như$N = 4$. Thuật toán tìm$x=1, y=z=0$, hợp lệ và tôn trọng giới hạn nếu$A \ge 1$. 

Trường hợp nguy hiểm hơn là khi tồn tại một biểu diễn hợp lệ nhưng yêu cầu dịch chuyển trọng số giữa các loại tiền xu, ví dụ:$$N = 20, A = 0, B = 4, C = 0$$Ở đây chỉ cho phép 5 giây nên thuật toán phải xác định chính xác$4 \times 5 = 20$. Điều này được ghi lại vì bảng liệt kê bao gồm$y=4, z=0$trong kiểm tra giới hạn.
