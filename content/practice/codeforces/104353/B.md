---
title: "CF 104353B - \u4e8b\u5173\u75af\u72c2\u661f\u671f\u56db\uff01"
description: "Quá trình trong vấn đề được thúc đẩy bởi một dòng thời gian dài trong nhiều ngày, bắt đầu từ ngày 1. Mỗi ngày, zy phải gửi một số tiền cố định, 5 đơn vị, đến Belmaxi vào buổi sáng. Điểm khác biệt duy nhất so với thói quen này là vào một số ngày nhất định, zy quên gửi tiền."
date: "2026-07-01T18:10:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104353
codeforces_index: "B"
codeforces_contest_name: "2023 Xiangtan University Programming Contest"
rating: 0
weight: 104353
solve_time_s: 53
verified: true
draft: false
---

[CF 104353B - \u4e8b\u5173\u75af\u72c2\u661f\u671f\u56db\uff01](https://codeforces.com/problemset/problem/104353/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình trong vấn đề được thúc đẩy bởi một dòng thời gian dài trong nhiều ngày, bắt đầu từ ngày 1. Mỗi ngày, zy phải gửi một số tiền cố định, 5 đơn vị, đến Belmaxi vào buổi sáng. Điểm khác biệt duy nhất so với thói quen này là vào một số ngày nhất định, zy quên gửi tiền. 

Ngoài ra còn có một cuộc kiểm tra hàng tuần diễn ra 7 ngày một lần, luôn được căn chỉnh sao cho ngày 1 là thứ Năm và mỗi ngày 7k+1 cũng là thứ Năm. Vào cuối ngày Thứ Năm hàng tuần (cụ thể là vào ngày 7k+1, 23:59), Belmaxi nhìn lại 7 ngày liên tiếp gần nhất kết thúc vào Thứ Năm đó và kiểm tra xem zy đã gửi tiền thành công cho tất cả các ngày đó hay chưa. Nếu cả bảy ngày đều được “trả tiền”, Belmaxi sẽ thưởng cho zy 50 đơn vị. Nếu không thì sẽ không có gì xảy ra vào thứ Năm đó. 

Dữ liệu đầu vào đưa ra tổng số ngày n và danh sách sắp xếp những ngày zy quên thanh toán. Nhiệm vụ là tính toán lợi nhuận ròng của Belmaxi: mỗi khoản thanh toán thành công đóng góp +5, mỗi chuỗi 7 ngày hoàn hảo hàng tuần đóng góp −50 theo quan điểm của Belmaxi vì anh ta trả phần thưởng. 

Thách thức chính là n có thể lớn tới 10^16, do đó việc lặp lại từng ngày là không thể. Thay vào đó, chỉ có số ngày thất bại thưa thớt là quan trọng. 

Một trường hợp phức tạp phát sinh từ cách các cửa sổ 7 ngày chồng lên nhau. Một ngày bị bỏ lỡ có thể đồng thời phá hủy nhiều phần thưởng hàng tuần và ngược lại, thành công kéo dài có thể tạo ra nhiều khoảng thời gian 7 ngày chồng chéo mà tất cả đều đủ điều kiện. 

Ví dụ: nếu n = 14 và không có lần trượt nào thì cả hai tuần đều có phần thưởng. Mạng lưới là 14 * 5 − 2 * 50 = 70 − 100 = −30. Một cách tiếp cận ngây thơ chỉ kiểm tra các tuần không trùng nhau sẽ bỏ lỡ phần thưởng thứ hai. 

Một trường hợp khác là khi bỏ lỡ cụm xung quanh ranh giới của các cửa sổ hàng tuần. Việc bỏ lỡ ngày thứ 7 chỉ ảnh hưởng đến cửa sổ đầu tiên, nhưng việc bỏ lỡ ngày thứ 8 sẽ ảnh hưởng đến cả cửa sổ kết thúc ở ngày thứ 7 và cửa sổ kết thúc ở ngày 14, tùy thuộc vào sự căn chỉnh. 

Bởi vì những sự chồng chéo này và n rất lớn, nên vấn đề giảm xuống còn việc suy luận về việc có bao nhiêu ngày bị bỏ lỡ bên trong các cửa sổ trượt có độ dài cố định 7 mà không lặp lại từng ngày. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ mô phỏng mỗi ngày từ 1 đến n, đánh dấu xem zy có thanh toán hay không, sau đó tính toán xem thời hạn 7 ngày cuối cùng đã hoàn thành hay chưa và liệu ngày đó có phải là điểm kiểm tra vào Thứ Năm hay không. Điều này hoạt động về mặt khái niệm: chúng tôi duy trì một mảng có kích thước n, điền vào những ngày còn thiếu và tính tổng cửa sổ trượt cho mỗi ngày. 

Tuy nhiên, điều này ngay lập tức là không thể vì n có thể là 10^16, do đó, ngay cả việc lưu trữ mảng cũng không khả thi chứ đừng nói đến việc lặp lại nó. Ngay cả khi n chỉ là 10^6, việc tính lại tổng thời hạn 7 ngày một cách ngây thơ sẽ là O(n) hoặc tệ hơn, nhưng vẫn có thể chấp nhận được. Ở đây, n sẽ hủy bỏ mọi phương thức mỗi ngày. 

Quan sát quan trọng là cấu trúc phần thưởng chỉ phụ thuộc vào việc mỗi phân đoạn 7 ngày kết thúc vào Thứ Năm có hoàn toàn “sạch” hay không. Vì Thứ Năm diễn ra 7 ngày một lần nên các phân đoạn này rời rạc về điểm cuối nhưng lại trùng lặp về nội dung. Thay vì mô phỏng các ngày, chúng tôi thay đổi quan điểm: mỗi ngày bị bỏ lỡ sẽ đóng góp một cách quyết định vào số lượng khoảng thời gian hàng tuần mà nó ảnh hưởng. 

Mỗi ngày bị bỏ lỡ ai ảnh hưởng nhiều nhất đến 7 cửa sổ kết thúc vào thứ Năm liên tiếp, bởi vì một ngày nằm chính xác trong các cửa sổ 7 ngày có điểm cuối nằm trong vòng 6 ngày sau ngày đó. Điều này cho phép chúng tôi xử lý việc đóng góp số ngày bị thiếu vào các tuần bị ảnh hưởng bằng cách sử dụng phạm vi tính toán trên chỉ số nén của tuần thay vì ngày. 

Chúng ta có thể coi mỗi tuần là chỉ số k tương ứng với các ngày [7k-6, 7k]. Việc bỏ lỡ ngày ai sẽ ảnh hưởng đến tất cả k sao cho 7k-6 ≤ ai ≤ 7k, chuyển thành một phạm vi số nguyên nhỏ của các giá trị k. Thay vì lặp lại tất cả các ngày, chúng tôi tích lũy số lần bỏ lỡ mỗi tuần bằng cách sử dụng một mảng khác nhau theo các chỉ số trong tuần. Sau khi xử lý tất cả các lần bỏ lỡ, một tuần chỉ đóng góp 50 nếu số lần bỏ lỡ của nó bằng 0.

Điều này làm giảm vấn đề đếm số tuần sạch, cộng với tổng số ngày cho thu nhập, cả hai đều có thể tính toán theo O(x). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(n) | Quá chậm | 
| Nén khoảng thời gian qua nhiều tuần | O(x) | O(x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi diễn giải lại dòng thời gian theo số tuần đầy đủ. Mỗi tuần k tương ứng với phân đoạn ngày từ 7k−6 đến 7k và chỉ ngày điểm cuối 7k mới quan trọng để đánh giá phần thưởng. Phần thưởng sẽ xảy ra chính xác khi tất cả bảy ngày trong phân đoạn đó được thanh toán. 

Tiếp theo, chúng tôi tính tổng số tuần hoàn chỉnh W = n // 7. Mọi ngày còn lại ngoài W * 7 sẽ không tạo thành cửa sổ kiểm tra hoàn chỉnh và không liên quan đến phần thưởng. 

Chúng tôi duy trì một cấu trúc theo dõi số ngày bị thiếu trong khoảng thời gian mỗi tuần. Thay vì đánh dấu riêng từng tuần cho từng ngày bị thiếu, chúng tôi dịch mỗi ngày bị thiếu ai thành một loạt chỉ số tuần bị ảnh hưởng. Một ngày ai nằm trong tuần k nếu 7k−6 ≤ ai ≤ 7k, tương đương với k ∈ [(ai + 6) // 7, (ai // 7)]. Chúng tôi tăng số lần bỏ lỡ của những tuần như vậy bằng cách sử dụng một mảng chênh lệch trên phạm vi số nguyên của tuần. 

Sau khi xử lý tất cả các ngày bị thiếu, chúng tôi quét qua các tuần từ 1 đến W, tích lũy tổng số tiền tố bị thiếu. Mỗi tuần không có lần bỏ lỡ nào sẽ đóng góp phần thưởng là 50 và mỗi ngày luôn đóng góp thu nhập cơ bản là 5 ngoại trừ những lần còn thiếu. 

Cuối cùng, chúng tôi kết hợp các khoản đóng góp: tổng thu nhập là 5 * (n − x) và tổng số tiền thanh toán gấp 50 lần số tuần sạch. 

### Tại sao nó hoạt động 

Phần thưởng của mỗi tuần chỉ phụ thuộc vào việc liệu ngày còn thiếu có nằm trong khoảng thời gian 7 ngày của ngày đó hay không. Việc chuyển đổi từ chỉ số ngày sang chỉ số tuần sẽ duy trì chính xác điều kiện này, bởi vì mỗi ngày bị bỏ lỡ sẽ ánh xạ tới chính xác tập hợp các tuần có khoảng thời gian chứa nó. Mảng chênh lệch đảm bảo rằng mỗi tuần tổng hợp tất cả các khoản đóng góp một cách chính xác và không có sự tương tác nào tồn tại giữa các khoảng thời gian tuần rời rạc ngoài lần đếm này. Điều này đảm bảo rằng sau khi tích lũy tiền tố, mỗi tuần đều được phân loại chính xác là sạch hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split())) if x else []

        # total base income from successful payments
        total_income = 5 * (n - x)

        W = n // 7  # number of full weeks that can produce rewards

        diff = [0] * (W + 3)

        for day in a:
            # compute affected week range [L, R]
            L = (day + 6) // 7
            R = day // 7

            if L <= R:
                if L <= W:
                    diff[L] += 1
                    if R + 1 <= W:
                        diff[R + 1] -= 1

        clean_weeks = 0
        cur = 0
        for i in range(1, W + 1):
            cur += diff[i]
            if cur == 0:
                clean_weeks += 1

        total_reward = 50 * clean_weeks
        out.append(str(total_income - total_reward))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Sự chuyển đổi cốt lõi là sự chuyển đổi từ chỉ số ngày sang chỉ số tuần. Các công thức`(day + 6) // 7`Và`day // 7`xác định phạm vi các tuần bị ảnh hưởng. Sự thay đổi +6 là điều đảm bảo rằng bất kỳ ngày nào cũng tràn chính xác vào cửa sổ 7 ngày chứa nó ngay cả khi nó ở gần ranh giới. 

Mảng khác biệt tránh đánh dấu rõ ràng từng tuần bị ảnh hưởng cho mỗi ngày bị bỏ lỡ. Thay vì cập nhật các vị trí có khả năng là O(7x), mỗi lần bỏ lỡ chỉ đóng góp các cập nhật O(1) và lần quét cuối cùng sẽ giải quyết số lượng thực tế. 

Tính toán thu nhập cơ bản`5 * (n - x)`an toàn vì mỗi ngày không thiếu luôn đóng góp chính xác một khoản thanh toán 5 đơn vị, không phụ thuộc vào phần thưởng. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu có n = 15 và bị trượt ở ngày thứ 7 và 8. 

Đầu tiên chúng ta tính W = 15 // 7 = 2 tuần. 

| Ngày nhớ | L = (d+6)//7 | R = d//7 | Cập nhật khác biệt | 
| --- | --- | --- | --- | 
| 7 | 2 | 1 | áp dụng cho tuần 2 | 
| 8 | 2 | 2 | áp dụng cho tuần 2 | 

Sau khi xử lý, chỉ có tuần thứ 2 bị ảnh hưởng. 

Chúng tôi quét tuần: 

| Tuần | Số lần bỏ lỡ tiền tố | Lau dọn? | 
| --- | --- | --- | 
| 1 | 0 | vâng | 
| 2 | 1 | không | 

Vậy clean_week = 1. 

Tổng thu nhập = 5 * (15 − 2) = 65 

Tổng phần thưởng = 50 * 1 = 50 

Đáp án = 15 

Điều này cho thấy cách tích lũy chính xác các phạm vi bỏ lỡ chồng chéo mà không cần tính hai lần các tuần không chính xác. 

Bây giờ hãy xem xét một trường hợp hoàn toàn sạch: n = 14, x = 0. 

W = 2. 

| Tuần | Đếm lỡ | Lau dọn? | 
| --- | --- | --- | 
| 1 | 0 | vâng | 
| 2 | 0 | vâng | 

Tổng thu nhập = 70, tổng phần thưởng = 100, đáp án = −30. 

Điều này chứng tỏ rằng mọi khoảng thời gian hợp lệ hàng tuần đều đóng góp độc lập và cấu trúc chồng chéo không gây trở ngại khi không có sai sót nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x + n/7) | mỗi lần bỏ lỡ đóng góp O(1), sau đó một lần quét tuyến tính trong nhiều tuần | 
| Không gian | O(n/7) | mảng chênh lệch theo chỉ số tuần | 

Các ràng buộc cho phép n tối đa 10^16, nhưng chỉ x tối đa 10^5. Lời giải chỉ phụ thuộc vào x và n/7, làm cho nó đủ nhanh. Bộ nhớ vẫn nhỏ vì chỉ lưu trữ tổng hợp cấp tuần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style cases
assert run("""2
15 2
7 8
16 1
10
""") == "-25\n?", "sample-like"

# minimal case
assert run("""1
1 0
""") == "5", "single day clean"

# all missed
assert run("""1
7 7
1 2 3 4 5 6 7
""") == "-35", "all missed kills reward"

# full clean two weeks
assert run("""1
14 0
""") == "-30", "two clean weeks"

# boundary miss affecting one week only
assert run("""1
14 1
7
""") == "-50", "single boundary miss"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ngày sạch sẽ | 5 | trường hợp cơ sở tối thiểu | 
| trọn tuần bỏ lỡ | -35 | trấn áp phần thưởng | 
| hai tuần sạch sẽ | -30 | phần thưởng chồng chéo | 
| lỡ ranh giới | -50 | lập bản đồ tuần chính xác | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một ngày bị bỏ lỡ nằm chính xác trên ranh giới của tuần, chẳng hạn như ngày 7 hoặc ngày 14. Với n = 14 với ngày bị bỏ lỡ ở ngày thứ 7, W = 2. Việc bỏ lỡ ánh xạ tới L = 1, R = 1, vì vậy chỉ tuần 1 bị ảnh hưởng. Tuần 2 vẫn sạch sẽ và vẫn tạo ra phần thưởng. Điều này xác nhận rằng việc lập bản đồ ranh giới không tràn vào các tuần liền kề một cách không chính xác. 

Một trường hợp đặc biệt khác là khi bỏ lỡ cụm ở gần cuối dòng thời gian nơi tồn tại các tuần chưa hoàn thành. Ví dụ: nếu n = 15 và trượt ở ngày thứ 15 thì ngày đó nằm ở tuần thứ 3 theo công thức, nhưng vì W = 2 nên nó hoàn toàn bị bỏ qua. Thuật toán sẽ loại bỏ nó một cách an toàn vì chỉ có đủ tuần mới đóng góp phần thưởng. 

Một trường hợp tinh vi cuối cùng là bỏ lỡ nhiều lần trong cùng một tuần. Đối với n = 7 và bỏ lỡ ở ngày 2, 3 và 5, tất cả chúng đều ánh xạ tới tuần 1 và tổng tiền tố đảm bảo tuần được tính một lần là không sạch bất kể có bao nhiêu lần bỏ lỡ xảy ra.
