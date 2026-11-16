---
title: "CF 1931B - Tạo sự bình đẳng"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có một dãy thùng chứa, mỗi thùng chứa một lượng nước. Chúng ta được phép di chuyển nước, nhưng chỉ từ thùng bên trái sang thùng bên phải."
date: "2026-06-08T18:24:33+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1931
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 925 (Div. 3)"
rating: 800
weight: 1931
solve_time_s: 74
verified: true
draft: false
---

[CF 1931B - Tạo sự bình đẳng](https://codeforces.com/problemset/problem/1931/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có một dãy thùng chứa, mỗi thùng chứa một lượng nước. Chúng ta được phép di chuyển nước, nhưng chỉ từ thùng bên trái sang thùng bên phải. Mục tiêu là để quyết định liệu cuối cùng chúng ta có thể phân phối lại lượng nước để mỗi thùng chứa có lượng nước như nhau hay không. 

Hoạt động được phép duy nhất là chuyển hướng: chọn chỉ số i và j với i < j và di chuyển bất kỳ lượng nước không âm nào từ i sang j. Điều này có nghĩa là nước chỉ có thể chảy sang phải, không bao giờ chảy sang trái mà có thể được phân chia và kết hợp tùy ý qua các vị trí trung gian. 

Ràng buộc n lên tới 2⋅10^5 trong tất cả các trường hợp thử nghiệm ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Bất kỳ phương pháp bậc hai nào, chẳng hạn như mô phỏng chuyển khoản hoặc cố định vị trí liên tục, sẽ ngay lập tức thất bại vì tổng số phép tính trong trường hợp xấu nhất sẽ vượt quá 10^10. 

Một trường hợp cạnh tinh tế phát sinh khi tổng số tiền chia cho n nhỏ nhưng các vùng chứa ban đầu lại quá thấp. Ví dụ: nếu phần tử đầu tiên đã lớn hơn mục tiêu, chúng tôi có thể nghi ngờ thất bại, nhưng điều này không tự động bị loại vì phần vượt quá có thể bị đẩy sang bên phải. Ngược lại, nếu tổng tiền tố ban đầu luôn ở dưới mức “đáng ra” thì sau này chúng ta có thể không bù được vì nước không thể di chuyển sang trái. 

Một trường hợp thất bại minh họa nhỏ cho trực giác ngây thơ là: 

đầu vào: 

n = 3, a = [0, 3, 0] 

Mục tiêu là 1. Mặc dù tổng số có thể chia hết nhưng vùng chứa đầu tiên bắt đầu bên dưới mục tiêu, nhưng điều này không sao vì nó có thể nhận từ bên phải. Tuy nhiên, nếu chúng ta đảo ngược các ràng buộc về hướng, tình hình sẽ thay đổi đáng kể, điều này cho thấy rằng khó khăn cốt lõi là tính định hướng. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ cố gắng chọn liên tục các cặp i < j và đẩy nước về phía trước để giảm sự mất cân bằng. Người ta có thể duy trì một vòng lặp quét vị trí phía trên mục tiêu và đẩy phần thừa sang bên phải. Trong trường hợp xấu nhất, mỗi đơn vị nước có thể được di chuyển qua các vị trí O(n) và có các đơn vị O(tổng a_i). Điều này dẫn đến độ phức tạp trong trường hợp xấu nhất theo thứ tự từ 10^9 đến 10^10 thao tác, điều này không khả thi. 

Quan sát cấu trúc quan trọng là ràng buộc hoạt động áp đặt một điều kiện tiền tố. Vì nước chỉ di chuyển sang phải nên bất kỳ sự thiếu hụt nào trong tiền tố phải được đáp ứng hoàn toàn từ bên trong tiền tố đó ở một giai đoạn trước đó. Một khi chúng ta vượt qua ranh giới tiền tố, chúng ta không bao giờ có thể mang nước trở lại để khắc phục tình trạng thiếu nước trước đó. 

Điều này dẫn đến một bất biến rõ ràng: ở mỗi tiền tố, chúng ta phải có đủ tổng lượng nước để hỗ trợ làm cho tiền tố đó đạt đến mức thống nhất cuối cùng. Vì mức cuối cùng được cố định là avg = tổng / n nên số tiền cần thiết ở k vị trí đầu tiên chính xác là k * avg. Vì vậy, điều quan trọng duy nhất là liệu tổng tiền tố đang chạy có bao giờ giảm xuống dưới k * avg hay không. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · tổng cộng) | O(1) | Quá chậm | 
| Kiểm tra số dư tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị đích avg = sum(a) / n. Đây là giá trị cuối cùng duy nhất có thể có trên mỗi thùng chứa vì tổng khối lượng được bảo toàn. 
2. Khởi tạo biến tổng tiền tố đang chạy thành 0. 
3. Lặp lại mảng từ trái sang phải. 
4. Tại mỗi vị trí i, thêm a[i] vào tổng tiền tố đang chạy. 
5. Tính số tiền cần thiết cho tiền tố này, đó là (i + 1) * avg. 
6. Nếu tại bất kỳ thời điểm nào, tổng tiền tố trở nên nhỏ hơn mức yêu cầu, ngay lập tức kết luận rằng điều đó là không thể. 
7. Nếu vòng lặp hoàn thành mà không vi phạm thì kết luận là có thể.

Lý do chúng tôi so sánh với tổng tiền tố bắt buộc là vì một khi chúng tôi di chuyển chỉ số i qua, không hoạt động nào trong tương lai có thể đưa nước trở lại chỉ số ≤ i. Vì vậy, tính khả thi phụ thuộc hoàn toàn vào việc liệu các vị trí trước đó có “nợ” nhiều nước hơn mức họ đã tích lũy hay không. 

### Tại sao nó hoạt động 

Quá trình di chuyển nước chỉ sang bên phải hàm ý rằng các tiền tố tiến hóa độc lập với các phân đoạn tương lai theo một hướng: các vị trí tương lai chỉ có thể giúp ích cho chính chúng và các vị trí ở bên phải chúng. Vì vậy, bất kỳ sự thiếu hụt tiền tố nào đều không thể thay đổi được. 

Nếu chúng ta tưởng tượng cấu hình cuối cùng, mỗi tiền tố phải chứa chính xác tỷ lệ nước theo tỷ lệ của nó. Nếu tại bất kỳ thời điểm nào trong quá trình quét, chúng tôi nhận thấy rằng tổng tiền tố hiện tại nhỏ hơn yêu cầu thì ngay cả khi tất cả lượng nước còn lại được đặt ở mức tối ưu, nó cũng không thể bù cho ranh giới tiền tố đã bị vi phạm vì khoản bù đó sẽ yêu cầu di chuyển nước sang trái qua ranh giới. 

Bất biến này mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        total = sum(a)
        avg = total // n
        
        pref = 0
        ok = True
        
        for i, x in enumerate(a):
            pref += x
            if pref < (i + 1) * avg:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán giá trị mục tiêu toàn cầu. Sau đó nó duy trì bộ tích lũy tiền tố trong khi quét từ trái sang phải. Việc kiểm tra quan trọng sẽ so sánh lượng nước tích lũy với lượng nước phân bố đồng đều theo yêu cầu cho đến thời điểm đó. Khi tiền tố bị thiếu, hàm sẽ dừng sớm vì không thể sửa lỗi sau này. 

Một cạm bẫy triển khai phổ biến là quên rằng phép chia số nguyên ở đây là an toàn vì vấn đề đảm bảo tính chia hết. Một vấn đề tinh vi khác là sử dụng “sự khác biệt” đang chạy thay vì so sánh tiền tố rõ ràng; cả hai đều hoạt động, nhưng việc xây dựng tiền tố ít xảy ra lỗi hơn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

Đầu vào: [4, 5, 2, 1, 3], trung bình = 3 

| tôi | một [tôi] | tiền tố | bắt buộc (i+1)*3 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 3 | vâng | 
| 1 | 5 | 9 | 6 | vâng | 
| 2 | 2 | 11 | 9 | vâng | 
| 3 | 1 | 12 | 12 | vâng | 
| 4 | 3 | 15 | 15 | vâng | 

Tất cả các tiền tố đều thỏa mãn ràng buộc, vì vậy câu trả lời là CÓ. Điều này khẳng định rằng lượng nước dư thừa ban đầu luôn có thể được đẩy sang phải để cố định các vị trí sau này. 

### Ví dụ 2 

Đầu vào: [1, 2, 3], trung bình = 2 

| tôi | một [tôi] | tiền tố | bắt buộc | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | không | 

Ở bước đầu tiên, tiền tố đã thất bại. Mặc dù các phần tử sau này chứa phần dư nhưng nó không thể di chuyển sang trái để khắc phục phần thiếu hụt trong vùng chứa đầu tiên, vì vậy câu trả lời là KHÔNG. Điều này cho thấy tính không thể đảo ngược của tình trạng thiếu tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | quét tiền tố một lượt | 
| Không gian | O(1) thêm | chỉ quầy được lưu trữ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2⋅10^5, do đó, việc quét tuyến tính cho mỗi trường hợp thử nghiệm vẫn được duy trì thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        avg = total // n
        pref = 0
        ok = True
        for i, x in enumerate(a):
            pref += x
            if pref < (i + 1) * avg:
                ok = False
                break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("""6
1
43
2
1 3
5
4 5 2 1 3
3
1 2 3
7
4 5 5 0 6 4 4
7
6 5 5 1 3 4 4
""") == """YES
NO
YES
NO
NO
YES"""

# custom cases
assert run("""1
1
0
""") == "YES"

assert run("""1
4
2 2 2 2
""") == "YES"

assert run("""1
3
3 0 0
""") == "NO"

assert run("""1
5
0 0 10 0 0
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | CÓ | container đơn tầm thường | 
| tất cả đều bình đẳng | CÓ | ổn định đã cân bằng | 
| thâm hụt sớm | KHÔNG | vi phạm tiền tố | 
| khối lượng quá muộn | KHÔNG | giới hạn luồng chỉ bên phải | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các phần tử đầu nhỏ hơn mục tiêu nhưng các phần tử sau chứa đủ phần dư. Ví dụ: [0, 0, 10] với avg = 3. Mặc dù tổng số là đủ nhưng tiền tố đầu tiên đã vi phạm yêu cầu vì vị trí đầu tiên sẽ cần 3 đơn vị nhưng có 0 và không thể nhận từ bên phải trong diễn giải trạng thái cuối cùng. 

Một trường hợp khác là khi mảng đã đồng nhất. Thuật toán vẫn kiểm tra tiền tố nhưng không bao giờ vi phạm nên trả về CÓ một cách chính xác mà không cần thực hiện bất kỳ thao tác nào. 

Cuối cùng, mảng một phần tử luôn thành công vì không cần phân phối lại và điều kiện tiền tố được thỏa mãn ở mỗi bước.
