---
title: "CF 1207A - Có Hai Loại Bánh Burger"
description: "Chúng ta được cung cấp một mô hình nhà hàng nhỏ trong đó mỗi truy vấn mô tả các nguyên liệu sẵn có và giá bán của hai loại bánh mì kẹp thịt. Một chiếc burger yêu cầu bánh bao và thịt bò, chiếc còn lại yêu cầu bánh bao và thịt gà."
date: "2026-06-13T16:19:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1207
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 71 (Rated for Div. 2)"
rating: 800
weight: 1207
solve_time_s: 178
verified: true
draft: false
---

[CF 1207A - Có hai loại bánh mì kẹp thịt](https://codeforces.com/problemset/problem/1207/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô hình nhà hàng nhỏ trong đó mỗi truy vấn mô tả các nguyên liệu sẵn có và giá bán của hai loại bánh mì kẹp thịt. Một chiếc burger yêu cầu bánh bao và thịt bò, chiếc còn lại yêu cầu bánh bao và thịt gà. Mỗi chiếc bánh mì kẹp thịt tiêu thụ chính xác hai chiếc bánh và mỗi loại tiêu thụ thành phần riêng của nó. 

Đối với mỗi truy vấn, chúng ta phải quyết định sẽ sản xuất bao nhiêu chiếc bánh mì kẹp thịt và bao nhiêu chiếc bánh mì kẹp thịt gà sao cho tổng số bánh được tôn trọng và tổng doanh thu được tối đa hóa. Nguồn tài nguyên hạn chế là bánh bao, chả bò và thịt gà cốt lết và mỗi truy vấn đều độc lập. 

Các ràng buộc rất nhỏ: mỗi số lượng tối đa là 100 và có tối đa 100 truy vấn. Điều này có nghĩa là bất kỳ giải pháp nào lên tới khoảng vài triệu thao tác đều không đáng kể để phù hợp với giới hạn thời gian. Ngay cả những cách tiếp cận thử tất cả các phân phối hợp lý của bánh mì kẹp thịt cũng khả thi, nhưng có một cấu trúc thậm chí còn đơn giản hơn để tránh hoàn toàn việc liệt kê. 

Một sai lầm ngây thơ trong vấn đề này là cho rằng chúng ta nên luôn ưu tiên loại bánh mì kẹp thịt đắt tiền hơn. Điều đó không phải lúc nào cũng đúng nếu chiếc bánh mì kẹp thịt đắt tiền tiêu thụ một nguồn tài nguyên hạn chế hơn. Ví dụ: nếu bánh có nhiều nhưng thịt bò thì khan hiếm trong khi thịt gà thì nhiều, việc ưu tiên bánh mì kẹp thịt có thể lãng phí những chiếc bánh lẽ ra có thể được dùng cho bánh mì kẹp thịt gà có giá trị hơn. 

Một trường hợp khó tinh tế khác xuất hiện khi cả hai loại burger đều có giá ngang nhau. Trong trường hợp đó, bất kỳ sự phân bổ nào tôn trọng các ràng buộc về nguồn lực đều là tối ưu, nhưng một chiến lược tham lam cam kết quá mức đối với một loại có thể làm giảm tính khả thi của loại kia mà không cải thiện được lợi nhuận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản là thử tất cả số lượng bánh mì kẹp thịt và bánh mì kẹp thịt gà có thể có. Vì mỗi chiếc bánh mì kẹp thịt tiêu thụ hai chiếc bánh mì kẹp thịt, nếu chúng ta cố định số lượng bánh mì kẹp thịt thì số bánh mì và nguyên liệu còn lại sẽ xác định số lượng bánh mì kẹp thịt gà mà chúng ta có thể sản xuất. Đối với mỗi lựa chọn số lượng bánh hamburger từ 0 đến giá trị khả thi tối đa của nó, chúng tôi tính toán số lượng bánh mì kẹp thịt gà tương ứng và đánh giá lợi nhuận. Điều này đúng vì nó khám phá tất cả các phân phối bánh hợp lệ giữa hai loại bánh mì kẹp thịt. 

Vấn đề là điều này vẫn liên quan đến việc lặp lại tới 100 lựa chọn cho mỗi truy vấn và sau đó tính toán các ràng buộc bên trong vòng lặp. Mặc dù điều này đã đủ nhỏ nhưng không cần thiết vì cấu trúc của vấn đề đơn giản hơn: cả hai loại bánh mì kẹp thịt đều tiêu thụ cùng một nguồn tài nguyên được chia sẻ (bánh mì), trong khi các thành phần khác là những hạn chế độc lập. Quan sát quan trọng là mỗi loại bánh mì kẹp thịt có thể được coi là tiêu thụ một công suất hạn chế và mối liên hệ thực sự duy nhất là hạn chế về bánh mì. 

Cái nhìn sâu sắc quan trọng là bánh là nút cổ chai được chia sẻ duy nhất. Mỗi chiếc bánh mì kẹp thịt tiêu thụ chính xác hai chiếc bánh, vì vậy tổng số bánh mì kẹp thịt bị giới hạn bởi`b // 2`. Sau khi chúng tôi quyết định sản xuất bao nhiêu chiếc bánh mì kẹp thịt của một loại, phần còn lại có thể được chỉ định cho loại kia theo giới hạn thành phần của nó. Vì chỉ có hai loại nên chiến lược tối ưu chỉ còn là thử hai cấu hình có ý nghĩa: ưu tiên bánh mì kẹp thịt trước hoặc ưu tiên bánh mì kẹp thịt gà trước. Một trong những lựa chọn tham lam này phải là tối ưu vì việc đổi loại bánh mì kẹp thịt này sang loại bánh mì kẹp thịt khác chỉ phụ thuộc vào lợi nhuận cận biên trên mỗi chiếc bánh được chia sẻ và không có tương tác nào ngoài điều đó. 

Điều này làm giảm vấn đề đánh giá hai phân bổ tham lam và lấy mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b) mỗi truy vấn | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán lợi nhuận tốt nhất bằng cách thử nghiệm cả hai ưu tiên có thể. 

1. Trước tiên hãy tính xem chúng ta có thể làm được bao nhiêu chiếc bánh hamburger nếu ưu tiên chúng. Điều này bị hạn chế bởi thịt bò và bánh bao, vì vậy nó`min(p, b // 2)`. Điều này thể hiện cam kết an toàn tối đa đối với bánh mì kẹp thịt mà không vi phạm các ràng buộc. 
2. Trừ số bánh dùng cho hamburger, để lại`b - 2 * hamburgers`bánh mì kẹp thịt gà. 
3. Tính bánh mì kẹp thịt gà như`min(f, remaining_buns // 2)`. Điều này đảm bảo chúng tôi không vượt quá nguồn cung gà hoặc số lượng bánh mì sẵn có. 
4. Tính tổng lợi nhuận như sau`hamburgers * h + chicken_burgers * c`. 
5. Lặp lại quy trình tương tự nhưng ưu tiên ngược lại: đầu tiên tối đa hóa số lượng bánh mì kẹp thịt gà, sau đó sử dụng số bánh mì còn lại cho bánh mì kẹp thịt. 
6. Kiếm lợi nhuận tối đa giữa hai chiến lược. 

Lý do chúng tôi kiểm tra rõ ràng cả hai hướng là quyết định tham lam phụ thuộc vào loại bánh mì kẹp thịt nào có quyền truy cập đầu tiên vào tài nguyên bánh mì được chia sẻ. 

### Tại sao nó hoạt động 

Sự kết hợp duy nhất giữa hai lựa chọn là hạn chế về bánh mì và cả hai loại bánh mì kẹp thịt đều tiêu thụ bánh mì như nhau. Điều này có nghĩa là bất kỳ giải pháp nào cũng tương ứng với việc chia tách`b // 2`khe cắm burger giữa hai loại dưới nắp độc lập`p`Và`f`. Lợi nhuận là tuyến tính theo số lượng bánh mì kẹp thịt của từng loại, do đó, giải pháp tối ưu phải nằm ở sự phân bổ cực đoan trong đó một loại được lấp đầy trước càng nhiều càng tốt và công suất còn lại được giao cho loại kia. Bất kỳ sự hoán đổi trung gian nào cũng chỉ có thể di chuyển các đơn vị giữa hai loại mà không thay đổi tổng số bánh và việc kiểm tra cả hai hướng cực đoan sẽ đảm bảo thu được số lượng tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(b, p, f, h, c):
    buns = b // 2

    # case 1: prioritize hamburgers
    ham1 = min(p, buns)
    buns_left1 = buns - ham1
    chick1 = min(f, buns_left1)
    profit1 = ham1 * h + chick1 * c

    # case 2: prioritize chicken burgers
    chick2 = min(f, buns)
    buns_left2 = buns - chick2
    ham2 = min(p, buns_left2)
    profit2 = chick2 * c + ham2 * h

    return max(profit1, profit2)

t = int(input())
for _ in range(t):
    b, p, f = map(int, input().split())
    h, c = map(int, input().split())
    print(solve_one(b, p, f, h, c))
```Giải pháp trước tiên là giảm số lượng bánh mì vào các khe bánh mì kẹp thịt có thể sử dụng được bằng cách chia số nguyên cho 2, vì mỗi bánh mì kẹp thịt tiêu thụ hai chiếc bánh mì. Sau đó nó mô phỏng cả hai thứ tự tham lam một cách rõ ràng. Mỗi đơn hàng đều tôn trọng cẩn thận cả giới hạn thành phần và dung tích bánh còn lại. 

Điểm tinh tế duy nhất là đảm bảo chúng tôi luôn tính toán lại các bánh còn lại sau khi chọn một loại trước. Việc quên bước này sẽ dẫn đến việc đếm quá mức và phân bổ không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
b = 15, p = 2, f = 3
h = 5, c = 10
```| Bước | Ham ưu tiên | Ưu tiên gà | 
| --- | --- | --- | 
| bánh bao | 7 | 7 | 
| loại đầu tiên | 2 giăm bông | 3 con gà | 
| bánh còn lại | 3 | 1 | 
| loại thứ hai | 1 con gà | 1 giăm bông | 
| lợi nhuận | 2_5 + 1_10 = 20 | 3_10 + 1_5 = 35 | 

Chiến lược tối ưu là chọn gà trước, mang lại 35. 

Điều này cho thấy rằng việc ưu tiên mặt hàng có giá cao hơn (gà) sẽ dẫn đến việc sử dụng tốt hơn số lượng bánh hạn chế trong cấu hình này. 

### Ví dụ 2 

đầu vào:```
b = 7, p = 5, f = 2
h = 10, c = 12
```| Bước | Ham ưu tiên | Ưu tiên gà | 
| --- | --- | --- | 
| bánh bao | 3 | 3 | 
| loại đầu tiên | 3 giăm bông | 2 con gà | 
| bánh còn lại | 0 | 1 | 
| loại thứ hai | 0 con gà | 0 giăm bông | 
| lợi nhuận | 30 | 24 | 

Ở đây, hamburger-first là tối ưu vì nó tiêu thụ bánh theo cách phù hợp hơn với khả năng sẵn có. 

Những ví dụ này xác nhận rằng việc đặt hàng chính xác phụ thuộc vào cả giá cả và tình trạng còn hàng chứ không chỉ riêng giá cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn thực hiện các phép tính số học theo thời gian không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì ngay cả ở 100 truy vấn, tổng số thao tác là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            b, p, f = map(int, input().split())
            h, c = map(int, input().split())

            buns = b // 2

            ham1 = min(p, buns)
            chick1 = min(f, buns - ham1)
            profit1 = ham1 * h + chick1 * c

            chick2 = min(f, buns)
            ham2 = min(p, buns - chick2)
            profit2 = chick2 * c + ham2 * h

            out.append(str(max(profit1, profit2)))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""3
15 2 3
5 10
7 5 2
10 12
1 100 100
100 100
""") == """40
34
0"""

# custom cases
assert run("""1
1 10 10
5 7
""") == "0", "minimum buns case"

assert run("""1
100 50 50
1 1
""") == "50", "symmetric low price case"

assert run("""1
10 100 0
10 1
""") == "50", "single type only"

assert run("""1
10 5 5
10 20
""") == "100", "prefer chicken due to price"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hộp bánh | 0 | xử lý bất khả thi | 
| tài nguyên đối xứng | 50 | phân bổ cân bằng | 
| chỉ một loại | 50 | sản xuất một chiều | 
| giá lệch | 100 | sự lựa chọn đặt hàng tham lam | 

## Vỏ cạnh 

Một hộp đựng bánh mì tối giản như`b = 1`ngay lập tức buộc câu trả lời về 0 vì không thể tạo thành bánh mì kẹp thịt. Thuật toán xử lý việc này thông qua`b // 2`, trở thành số 0, đảm bảo cả hai chiến lược đều không sản xuất ra bánh mì kẹp thịt và không có bánh mì kẹp thịt gà. 

Ví dụ: trường hợp một thành phần bằng 0`p = 0`, buộc tất cả sản xuất phải chuyển sang bánh mì kẹp thịt gà. Trong thuật toán,`min(p, buns)`chính xác sẽ trở thành 0, vì vậy chiến lược đầu tiên tự nhiên gán toàn bộ công suất cho gà mà không yêu cầu xử lý đặc biệt. 

Khi cả hai giá bánh mì kẹp thịt bằng nhau, thuật toán vẫn hoạt động vì cả hai đơn hàng tham lam đều tạo ra sự phân bổ hợp lệ. Vì lợi nhuận là đối xứng nên việc đặt hàng đều mang lại kết quả như nhau và đảm bảo tính chính xác tối đa mà không cần logic ràng buộc.
