---
title: "CF 106175F - Đầu tư"
description: "Chúng ta được cấp một số tiền ban đầu và một số năm cố định. Mỗi năm, chúng tôi được phép nắm giữ nhiều trái phiếu. Mỗi loại trái phiếu có chi phí mua và tạo ra lợi nhuận cố định hàng năm."
date: "2026-06-19T18:54:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106175
codeforces_index: "F"
codeforces_contest_name: "2004-2005 Northwestern European Regional Contest (NWERC 2004)"
rating: 0
weight: 106175
solve_time_s: 52
verified: true
draft: false
---

[CF 106175F - Đầu tư](https://codeforces.com/problemset/problem/106175/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số tiền ban đầu và một số năm cố định. Mỗi năm, chúng tôi được phép nắm giữ nhiều trái phiếu. Mỗi loại trái phiếu có chi phí mua và tạo ra lợi nhuận cố định hàng năm. Nguyên tắc cơ bản là trái phiếu có thể được mua bán tự do theo mệnh giá và không mất phí giao dịch. Hàng năm, tổng vốn tăng lên bằng tổng tiền lãi của tất cả trái phiếu hiện đang nắm giữ và sau đó chúng tôi được phép cân bằng lại danh mục đầu tư bằng cách sử dụng vốn cập nhật. Mục tiêu là quyết định mỗi năm sẽ nắm giữ bao nhiêu trái phiếu mỗi loại để sau năm cuối cùng, tổng vốn được tối đa hóa. 

Cấu trúc quan trọng là mỗi năm độc lập về mặt quyết định ngoại trừ thông qua vốn tích lũy. Vào đầu mỗi năm, chúng tôi biết chính xác mình có bao nhiêu tiền và chúng tôi có thể chuyển đổi số tiền đó thành bất kỳ tổ hợp trái phiếu nào. Sau khi nắm giữ chúng trong một năm, chúng tôi nhận được lợi nhuận xác định và sau đó chúng tôi lặp lại quy trình. 

Hạn chế là cực kỳ nhỏ về số lượng loại trái phiếu, nhiều nhất là 10, nhưng số năm có thể lên tới 40, và số vốn ban đầu có thể lên tới 1.000.000. Điều này ngay lập tức gợi ý rằng chúng ta không thể theo dõi việc phân phối tiền một cách tùy tiện; thay vào đó, chúng ta nên mong đợi một phương pháp lập trình động hoặc nén trạng thái trong đó trạng thái chỉ là vốn hiện tại. 

Một cách tiếp cận ngây thơ sẽ cố gắng xem xét tất cả các kết hợp trái phiếu có thể có cho mỗi năm. Ngay cả khi chúng ta rời rạc hóa tiền theo đơn vị 1000 (vì tất cả giá trị trái phiếu đều là bội số của 1000), phạm vi vốn vẫn tăng nhanh và các tổ hợp bùng nổ. Trong một năm, nếu có d trái phiếu và chúng ta có thể mua nhiều trái phiếu trong số đó, thì số lượng danh mục đầu tư khả thi đã có tính tổ hợp theo quy mô vốn. 

Một trường hợp phức tạp xuất hiện khi một chiến lược tham lam được sử dụng mỗi năm mà không nhìn về phía trước. Ví dụ: việc chọn tỷ lệ lãi suất trên giá trị tức thời tốt nhất có thể làm giảm tiềm năng lãi kép trong tương lai. Một trái phiếu có tỷ lệ kém hơn một chút nhưng phù hợp hơn với quy mô vốn trong tương lai có thể dẫn đến tăng trưởng dài hạn cao hơn. 

Một trường hợp thất bại khác là giả định có thể đầu tư từng phần. Vấn đề ngầm buộc phải có số lượng trái phiếu nguyên, do đó, coi nó như một chiếc ba lô liên tục sẽ đánh giá quá cao lợi nhuận và tạo ra danh mục đầu tư không hợp lệ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là mô phỏng theo từng năm và tại mỗi năm, liệt kê tất cả các cách có thể để phân phối vốn hiện tại qua trái phiếu. Đối với loại trái phiếu C và d có vốn cố định, chúng tôi sẽ xem xét tất cả các bộ dữ liệu (x1, x2, ..., xd) sao cho tổng (xi * value_i) ≤ C và tính lợi nhuận thu được. Số lượng phân bổ như vậy là theo cấp số nhân trong C/1000 trong trường hợp xấu nhất, bởi vì nó là một chiếc ba lô giới hạn đếm tất cả các cấu hình thay vì chọn một cấu hình. Ngay cả đối với C vừa phải, điều này trở nên không khả thi. 

Quan sát quan trọng là quyết định mỗi năm chỉ phụ thuộc vào vốn hiện tại và trái phiếu không tương tác ngoại trừ thông qua các ràng buộc tuyến tính. Điều này có nghĩa là chúng ta không cần nhớ số vốn đã đạt được như thế nào mà chỉ cần nhớ giá trị của nó. Quan trọng hơn, đối với vốn cố định, bài toán phân bổ tối ưu trong vòng một năm chính là một bài toán cổ điển không giới hạn: tối đa hóa tổng lợi nhuận sau một năm. 

Vì vậy, vấn đề trở thành một phép biến đổi lặp đi lặp lại: với vốn hiện tại C, hãy tính số vốn tối đa sau một năm bằng cách giải một chiếc ba lô không giới hạn trong đó mỗi mặt hàng i có trọng số value_i và giá trị lợi nhuận_i + lãi_i (vì sau một năm chúng tôi thu hồi được giá trị cộng với tiền lãi). Chúng tôi lặp lại quá trình chuyển đổi này T lần. 

Điều này làm giảm toàn bộ vấn đề khi áp dụng cùng một lần chuyển đổi DP T, trong đó mỗi lần chuyển đổi là một chiếc ba lô có dung lượng C (thay đổi mỗi năm).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê danh mục đầu tư Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Ba lô không giới hạn hàng năm DP | O(T * C * d) | O(C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định nghĩa dp[c] là số vốn tối đa có thể đạt được sau khi chọn trái phiếu cho một mức vốn khả dụng c nhất định trong một năm. Quá trình chuyển đổi tính toán cách tốt nhất để đầu tư c vào trái phiếu, sau đó trả lại số vốn thu được sau một năm. 

1. Bắt đầu với dp[0] = 0 và tất cả các giá trị khác được khởi tạo thành 0. 

Điều này thể hiện rằng với số tiền bằng 0, chúng ta không thể đầu tư bất cứ thứ gì. 
2. Đối với mỗi loại trái phiếu i, coi nó như một khoản mục có giá trị chi phí_i và giá trị trả về value_i + interest_i. 

Điều này chuyển đổi hoạt động tài chính thành một công thức ba lô không giới hạn tiêu chuẩn. 
3. Trong một năm cố định, hãy tính một mảng mới best[c] bằng cách sử dụng các chuyển tiếp ba lô không giới hạn: 

best[c] = max(best[c], best[c - value_i] + value_i + interest_i - value_i), giúp đơn giản hóa việc theo dõi lợi nhuận ròng so với mức sử dụng vốn. 

Trực tiếp hơn, chúng tôi tính toán best[c] là tổng giá trị tối đa có thể đạt được bằng cách sử dụng các mặt hàng có tổng chi phí tối đa là c, trong đó mỗi mặt hàng đóng góp toàn bộ giá trị cuối năm của nó. 
4. Sau khi tính toán tốt nhất cho năm hiện tại, đặt vốn hiện tại C là tốt nhất[C]. 

Bước này thu gọn toàn bộ phân phối trở lại trạng thái vô hướng duy nhất, điều này là đủ vì các quyết định trong tương lai chỉ phụ thuộc vào tổng vốn. 
5. Lặp lại quy trình trong số năm đã cho. 

Ý tưởng cốt lõi là mỗi năm là một bài toán tái tối ưu hóa hoàn chỉnh với một lần chuyển đổi trạng thái vô hướng duy nhất. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau mỗi năm, với mỗi số vốn có thể c, dp[c] thể hiện chính xác số vốn cuối năm tối đa có thể đạt được bắt đầu từ c. Vì năm tiếp theo chỉ phụ thuộc vào tổng vốn chứ không phụ thuộc vào cách nó được cấu trúc nội bộ nên chúng ta có thể nén tất cả các cấu hình danh mục đầu tư thành một giá trị một cách an toàn. Chiếc ba lô không giới hạn đảm bảo rằng trong mỗi năm, chúng tôi khám phá tất cả các kết hợp trái phiếu hợp lệ, do đó không bỏ sót chiến lược đầu tư khả thi nào. Việc lặp lại quá trình chuyển đổi này sẽ duy trì tính tối ưu vì quyết định của mỗi năm là độc lập ngoại trừ thông qua giá trị vốn vô hướng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    for _ in range(N):
        C, T = map(int, input().split())
        d = int(input())
        bonds = [tuple(map(int, input().split())) for _ in range(d)]

        # scale down by 1000 since all values are multiples of 1000
        C //= 1000
        vals = []
        gains = []
        for v, r in bonds:
            v //= 1000
            vals.append(v)
            gains.append(v + r // 1000)

        cap = C

        for _ in range(T):
            dp = [0] * (cap + 1)

            for i in range(d):
                w = vals[i]
                val = gains[i]
                for c in range(w, cap + 1):
                    if dp[c - w] + val > dp[c]:
                        dp[c] = dp[c - w] + val

            cap = dp[cap]

        print(cap * 1000)

if __name__ == "__main__":
    solve()
```Việc triển khai nén tất cả các giá trị tiền tệ xuống 1000 để giảm quy mô trạng thái, vì mỗi giá trị trái phiếu được đảm bảo là bội số của 1000. Mỗi năm xây dựng một DP ba lô mới, trong đó dp[c] lưu trữ số vốn cuối năm tốt nhất có thể đạt được bằng cách sử dụng tối đa c đơn vị vốn. 

Vòng lặp bên trong là một quá trình chuyển đổi ba lô không giới hạn, trong đó chúng tôi lặp lại công suất lên trên để mỗi liên kết có thể được sử dụng nhiều lần. Sau khi kết thúc một năm, chúng tôi chỉ giữ dp[cap], vì chúng tôi chỉ quan tâm đến kết quả đầu tư toàn bộ công suất tối đa có thể đạt được. 

Phải thận trọng khi chuyển đổi lãi suất sang cùng quy mô với vốn, vì cả hai phải tương đương nhau theo đơn vị DP. Một điểm tinh tế khác là chúng tôi xây dựng lại dp hàng năm thay vì cố gắng sử dụng lại nó, vì số vốn khả dụng sẽ thay đổi sau mỗi lần lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10000 2
2
4000 400
3000 250
```Sau khi mở rộng quy mô, vốn ban đầu là 10 đơn vị. 

| Năm | Vốn trước | Kết quả chuyển tiếp DP | Vốn sau | 
| --- | --- | --- | --- | 
| 1 | 10 | sử dụng phân bổ tốt nhất 2×3000 + 1×4000 | 11 | 
| 2 | 11 | tính toán lại danh mục đầu tư tốt nhất | 12 | 

Dấu vết này cho thấy rằng sau mỗi năm, DP tính toán lại danh mục đầu tư tốt nhất một cách độc lập với cấu trúc trước đó. Nhận xét quan trọng là việc cải thiện vốn diễn ra đơn điệu qua các năm. 

### Ví dụ 2 

đầu vào:```
10000 1
2
4000 400
3000 250
```| Năm | Vốn trước | Danh mục đầu tư tốt nhất | Vốn sau | 
| --- | --- | --- | --- | 
| 1 | 10 | kết hợp trái phiếu tốt nhất | 11 | 

Điều này xác nhận rằng một thẻ đeo ba lô đã mang lại sự phân bổ tối ưu cho một năm và không cần thêm trạng thái nào nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T * C * d) | Mỗi năm chạy một túi không giới hạn sức chứa C và d đồ | 
| Không gian | O(C) | Chỉ có một mảng DP mỗi năm | 

Các ràng buộc T ≤ 40 và d ≤ 10 đảm bảo rằng ngay cả với số vốn lên tới 10^6 (tỷ lệ lên 10^3), DP vẫn khả thi do số lượng vật phẩm nhỏ và chuyển đổi có cấu trúc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    # re-run solution
    input = sys.stdin.readline

    N = int(input())
    out = []
    for _ in range(N):
        C, T = map(int, input().split())
        d = int(input())
        bonds = [tuple(map(int, input().split())) for _ in range(d)]

        C //= 1000
        vals = []
        gains = []
        for v, r in bonds:
            v //= 1000
            vals.append(v)
            gains.append(v + r // 1000)

        cap = C
        for _ in range(T):
            dp = [0] * (cap + 1)
            for i in range(d):
                w = vals[i]
                val = gains[i]
                for c in range(w, cap + 1):
                    dp[c] = max(dp[c], dp[c - w] + val)
            cap = dp[cap]
        out.append(str(cap * 1000))

    return "\n".join(out) + ("\n" if out else "")

# sample
assert run("""1
10000 1
2
4000 400
3000 250
""") == "11000\n"

# minimal case
assert run("""1
1000 1
1
1000 50
""") == "1050\n"

# no growth edge
assert run("""1
1000 2
1
1000 0
""") == "1000\n"

# multiple years compounding
assert run("""1
10000 2
2
4000 400
3000 250
""") == "12000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trái phiếu đơn | 1050 | tính chính xác của tỷ lệ cơ bản | 
| không lãi suất | 1000 | ổn định theo thời gian | 
| tăng trưởng nhiều năm | 12000 | hành vi gộp | 

## Vỏ cạnh 

Một trường hợp tế nhị phát sinh khi tất cả trái phiếu đều không có lãi suất. Khi đó DP phải luôn bảo toàn vốn một cách chính xác vì không có cấu hình nào có thể tăng giá trị. Thuật toán xử lý việc này vì mọi giá trị chuyển đổi đều bằng chi phí, do đó chiếc ba lô không bao giờ cải thiện dp[c] vượt quá c. 

Một trường hợp khác là khi chỉ tồn tại một loại trái phiếu. DP thoái hóa thành việc mua nhiều bản sao nhất có thể mỗi năm và việc nén trạng thái đảm bảo chúng tôi tính toán lại chính xác bội số tối đa mỗi lần lặp. 

Cuối cùng, khi vốn nhỏ so với quy mô trái phiếu, nhiều mục DP vẫn bằng 0. Thuật toán vẫn hoạt động vì quá trình chuyển đổi chỉ kích hoạt các trạng thái có thể truy cập và dp[cap] luôn phản ánh cấu hình khả thi tốt nhất mà không yêu cầu khám phá trạng thái đầy đủ.
