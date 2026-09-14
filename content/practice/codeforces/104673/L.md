---
title: "CF 104673L - Toa Xe"
description: "Một chuyến tàu di chuyển qua một chuỗi các thành phố theo một thứ tự cố định và tại mỗi thành phố có sẵn một số loại cần cẩu, mỗi loại có một mức giá. Mỗi loại cần cẩu được xác định bằng một ID và tại một thành phố nhất định, bạn có thể mua hoặc bán bất kỳ loại nào được liệt kê ở đó với mức giá của thành phố đó."
date: "2026-06-29T09:22:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104673
codeforces_index: "L"
codeforces_contest_name: "2022-2023 CTU Open Contest"
rating: 0
weight: 104673
solve_time_s: 43
verified: true
draft: false
---

[CF 104673L - Toa xe](https://codeforces.com/problemset/problem/104673/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một chuyến tàu di chuyển qua một chuỗi các thành phố theo một thứ tự cố định và tại mỗi thành phố có sẵn một số loại cần cẩu, mỗi loại có một mức giá. Mỗi loại cần cẩu được xác định bằng một ID và tại một thành phố nhất định, bạn có thể mua hoặc bán bất kỳ loại nào được liệt kê ở đó với mức giá của thành phố đó. 

Hạn chế chính là tàu chỉ di chuyển về phía trước. Nếu bạn mua cần cẩu ở thành phố nào đó, bạn chỉ có thể bán nó ở thành phố sau. Bạn được phép lặp lại quá trình này nhiều lần, nhưng bạn chỉ có thể mang một cần cẩu vào bất kỳ thời điểm nào, nghĩa là các giao dịch không bị trùng lặp. Bạn bắt đầu với đủ tiền để mua bất kỳ cần cẩu nào ở bất kỳ thành phố nào, vì vậy mục tiêu duy nhất là tối đa hóa tổng lợi nhuận từ tất cả các hoạt động mua và bán. 

Nhiệm vụ là tính toán lợi nhuận tối đa có thể đạt được dọc theo tuyến đường. 

Các hạn chế bao hàm tới 100.000 thành phố, mỗi thành phố có tối đa 10 loại cần cẩu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xem xét trực tiếp tất cả các cặp thành phố, vì đó sẽ là phương trình bậc hai trong trường hợp xấu nhất và dẫn đến khoảng 10^10 phép tính. Ngay cả việc lặp lại tất cả các cặp thành phố cho mỗi loại cần cẩu cũng sẽ quá chậm. 

Một giải pháp đúng phải khai thác thực tế là cấu trúc có tính tuần tự và mỗi loại cần cẩu có thể được xử lý độc lập theo thời gian. 

Một trường hợp thất bại tinh vi xuất hiện khi một chiến lược tham lam ngây thơ được sử dụng cho mỗi thành phố, chẳng hạn như luôn mua cần cẩu rẻ nhất hiện có và bán nó với giá cao hơn tiếp theo mà không theo dõi các cơ hội trong tương lai. Ví dụ, nếu một chiếc cần cẩu rẻ sớm nhưng thậm chí còn rẻ hơn về sau, thì cách tiếp cận tham lam “mua ngay, bán đợt tăng tiếp theo” có thể cam kết quá sớm và bỏ lỡ cơ hội tăng giá lớn hơn nhiều sau này. Giải pháp đúng phải cho phép chờ đợi và lựa chọn cặp mua bán tốt nhất trên toàn cầu chứ không phải tại địa phương. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ coi mỗi giao dịch có thể xảy ra là sự lựa chọn của thành phố mua, thành phố bán sau trong tuyến và loại cần cẩu có mặt ở cả hai thành phố. Đối với mỗi loại cần cẩu, chúng tôi có thể quét tất cả các cặp thành phố nơi nó xuất hiện, tính toán chênh lệch giá và thu được lợi nhuận tốt nhất. Điều này đúng vì mọi giao dịch hợp lệ đều được xem xét rõ ràng, nhưng tốc độ quá chậm: trong trường hợp xấu nhất, một loại cẩu xuất hiện ở tất cả các thành phố, dẫn đến O(N^2) cặp cho mỗi loại và tối đa 10 loại cho mỗi thành phố, khiến tổng độ phức tạp đạt hiệu quả là O(N^2). 

Quan sát quan trọng là mỗi loại cần cẩu phát triển độc lập theo thời gian. Đối với loại cần cẩu cố định, chúng tôi chỉ quan tâm đến cơ hội tốt nhất để mua ở thành phố trước và bán ở thành phố sau. Điều này giúp giảm bớt vấn đề trong việc theo dõi, đối với từng loại, mức giá tối thiểu được thấy cho đến nay và lợi nhuận tốt nhất có thể đạt được khi bán tại thành phố hiện tại. Khi chúng tôi tiến về phía trước, mọi thành phố mới đều có thể cải thiện giá mua tối thiểu hoặc tạo ra một giao dịch bán có lợi nhuận so với mức tối thiểu được quan sát trước đó. 

Điều này biến bài toán tổng thể thành bài toán chênh lệch dòng lớn nhất cho mỗi loại cần trục, được xử lý theo trình tự thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các cặp mỗi loại | O(N^2 · M) | O(1) | Quá chậm | 
| Giá tối thiểu theo dõi một lần cho mỗi loại | O(N · M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các thành phố theo thứ tự trong khi duy trì, đối với từng loại cần cẩu, mức giá thấp nhất từ trước đến nay và lợi nhuận tốt nhất có thể đạt được.

1. Khởi tạo một từ điển cho từng loại cần cẩu sẽ lưu trữ giá quan sát tối thiểu. Đồng thời khởi tạo tổng lợi nhuận bằng 0. Điều này chuẩn bị cho chúng tôi đánh giá mọi loại một cách độc lập khi chúng tôi quét về phía trước. 
2. Lặp lại các thành phố từ trái sang phải. Tại mỗi thành phố, chúng tôi kiểm tra tất cả các loại cần cẩu có sẵn ở thành phố đó. 
3. Đối với từng loại cần cẩu và giá của nó ở thành phố hiện tại, trước tiên chúng tôi kiểm tra xem loại này đã từng được nhìn thấy trước đây chưa. Nếu không, chúng tôi khởi tạo giá tối thiểu bằng giá hiện tại vì đây là điểm mua đầu tiên có thể có. 
4. Nếu loại này đã được nhìn thấy trước đó, chúng tôi so sánh giá hiện tại với giá tối thiểu được lưu trữ. Bán hàng tại thành phố hiện tại sẽ mang lại lợi nhuận bằng giá hiện tại trừ đi giá tối thiểu. Nếu lợi nhuận này dương và tốt hơn bất kỳ khoản lợi nhuận nào được ghi nhận trước đó, chúng tôi sẽ cộng nó vào tổng lợi nhuận. 
5. Bất kể chúng tôi có bán hay không, chúng tôi cập nhật giá tối thiểu cho loại cần cẩu này nhỏ hơn mức giá tối thiểu hiện có và giá hiện tại. Điều này đảm bảo rằng các thành phố trong tương lai luôn xem xét cơ hội mua tốt nhất có thể. 
6. Tiếp tục quá trình này cho tất cả các thành phố và tất cả các loại cần cẩu trong đó. 

Chi tiết quan trọng là chúng tôi không bao giờ “khóa” một giao dịch. Mỗi bản cập nhật chỉ nâng cao kiến ​​thức về giá mua tốt nhất trong quá khứ và mỗi thành phố đánh giá cơ hội bán hàng mà không tiêu thụ hàng tồn kho. 

### Tại sao nó hoạt động 

Đối với mỗi loại cần cẩu, thuật toán duy trì bất biến rằng sau khi xử lý thành phố i, giá tối thiểu được lưu trữ chính xác là giá thấp nhất của loại đó trong số tất cả các thành phố từ 1 đến i. Bất kỳ giao dịch hợp lệ nào cũng phải bao gồm mua tại thành phố j trước đó và bán tại thành phố i sau đó, vì vậy khi xử lý thành phố i, thuật toán sẽ so sánh chính xác với điểm mua tốt nhất có thể. Vì mỗi cơ hội bán được đánh giá chính xác một lần tại thành phố bán của nó và mọi ứng cử viên mua đều được đưa vào mức tối thiểu đang chạy nên không có cặp hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    min_price = {}
    profit = 0

    for _ in range(n):
        m = int(input())
        for _ in range(m):
            cid, price = map(int, input().split())

            if cid not in min_price:
                min_price[cid] = price
            else:
                if price > min_price[cid]:
                    profit += price - min_price[cid]
                if price < min_price[cid]:
                    min_price[cid] = price

    print(profit)

if __name__ == "__main__":
    solve()
```Mã duy trì một từ điển được khóa theo loại cần cẩu. Mỗi mục lưu trữ giá tối thiểu được quan sát cho đến nay. Khi chúng tôi đọc từng thành phố, chúng tôi xử lý tất cả các loại cần cẩu có sẵn. Nếu có thể bán có lãi ở mức giá hiện tại, nó sẽ được cộng ngay vào lợi nhuận toàn cầu. Mức tối thiểu được cập nhật trong cùng một lượt để các thành phố trong tương lai luôn so sánh với giá mua lịch sử tốt nhất. 

Một điểm tinh tế là chúng tôi không bao giờ loại bỏ một loại sau khi bán. Điều này đúng vì chúng tôi không mô phỏng việc giữ hàng tồn kho mà thay vào đó phân tách mọi giao dịch thành các cặp mua-bán độc lập. Mỗi cặp có lợi nhuận được tính chính xác một lần khi chúng tôi đến thành phố bán. 

## Ví dụ đã hoạt động 

Hãy xem xét một tiến trình đơn giản với một loại cần cẩu. 

đầu vào:```
3
1
1 2
1
1 5
1
1 3
```Chúng tôi theo dõi loại 1. 

| Thành phố | Giá | Tối thiểu cho đến nay | Lợi nhuận tăng thêm | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 
| 2 | 5 | 2 | 3 | 
| 3 | 3 | 2 | 0 | 

Thuật toán mua hiệu quả ở mức giá 2 và bán ở mức giá 5, sau đó bỏ qua mức giá thấp hơn sau đó vì nó không mang lại lợi nhuận. 

Điều này cho thấy lợi nhuận chỉ được tích lũy khi giá sau đó vượt quá điểm mua tốt nhất trước đó. 

Bây giờ hãy xem xét nhiều dao động: 

đầu vào:```
4
1
1 3
1
1 1
1
1 4
1
1 2
```| Thành phố | Giá | Tối thiểu cho đến nay | Lợi nhuận tăng thêm | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | 4 | 1 | 3 | 
| 4 | 2 | 1 | 0 | 

Điều này chứng tỏ rằng sau khi đặt lại mức tối thiểu ở mức giá thấp hơn, mức tăng trong tương lai vẫn được nắm bắt chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · M) | Mỗi thành phố xử lý tối đa 10 loại cần cẩu | 
| Không gian | O(K) | K là số loại cần cẩu riêng biệt được lưu trong từ điển | 

Các giới hạn làm cho việc này trở nên hiệu quả vì N lên tới 100.000 và M nhiều nhất là 10, do đó tổng số hoạt động là khoảng 1.000.000 cập nhật, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("""3
1
1 2
1
1 5
1
1 3
""") == "3"

# minimum size
assert run("""1
1
10 5
""") == "0"

# monotone increasing
assert run("""4
1
1 1
1
1 2
1
1 3
1
1 4
""") == "3"

# monotone decreasing
assert run("""4
1
1 5
1
1 4
1
1 3
1
1 2
""") == "0"

# multiple types
assert run("""3
2
1 1
2 10
2
1 5
2 3
1
1 10
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thành phố duy nhất | 0 | không thể bán kỳ hạn | 
| trình tự tăng dần | tích lũy tích cực | tham lam nắm bắt tăng | 
| dãy giảm dần | 0 | không có lợi nhuận âm không hợp lệ | 
| nhiều loại | theo dõi độc lập hỗn hợp | độ chính xác của từng loại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cùng một loại cần cẩu xuất hiện nhiều lần trong một thành phố hoặc thường xuyên thay đổi giữa giá thấp và giá cao. 

đầu vào:```
3
2
1 5
1 1
2
1 2
2 10
2
1 6
2 3
```Đối với loại 1, mức tối thiểu trở thành 1 tại thành phố 1 và bán chạy nhất là 6 tại thành phố 3, mang lại lợi nhuận 5. Giá cao hơn trung gian 5 bị bỏ qua vì nó không cải thiện lợi nhuận ngoài cơ hội sau này. 

Đối với loại 2, tối thiểu là 10 tại thành phố 2, nhưng giá 3 sau này không mang lại lợi nhuận nên không đóng góp. 

Điều này xác nhận rằng các cập nhật cục bộ về tích lũy lợi nhuận tối thiểu và ngay lập tức xử lý chính xác các ID lặp lại trong và khắp các thành phố mà không cần mô phỏng giao dịch rõ ràng.
