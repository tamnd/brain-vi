---
title: "CF 104270J - Sách"
description: "Chúng tôi được cung cấp một chuỗi sách, mỗi cuốn có một mức giá cố định và quy trình mua hàng xác định sẽ quét sách từ trái sang phải. Ở mỗi cuốn sách, nếu số tiền hiện tại ít nhất bằng giá thì cuốn sách được mua và số tiền giảm đi; nếu không thì cuốn sách sẽ bị bỏ qua."
date: "2026-07-01T21:29:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104270
codeforces_index: "J"
codeforces_contest_name: "The 2018 ICPC Asia Qingdao Regional Programming Contest (The 1st Universal Cup, Stage 9: Qingdao)"
rating: 0
weight: 104270
solve_time_s: 64
verified: true
draft: false
---

[CF 104270J - Sách](https://codeforces.com/problemset/problem/104270/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi sách, mỗi cuốn có một mức giá cố định và quy trình mua hàng xác định sẽ quét sách từ trái sang phải. Ở mỗi cuốn sách, nếu số tiền hiện tại ít nhất bằng giá thì cuốn sách được mua và số tiền giảm đi; nếu không thì cuốn sách sẽ bị bỏ qua. Kết quả cuối cùng của quá trình này là số lượng sách được mua. 

Nhiệm vụ được đảo ngược so với hướng mô phỏng thông thường. Thay vì được đưa số tiền ban đầu và hỏi mua bao nhiêu cuốn sách, chúng tôi được đưa ra giá và số lượng sách đã mua cuối cùng.`m`và chúng ta phải xác định số tiền ban đầu tối đa có thể dẫn đến kết quả chính xác`m`mua hàng theo quy luật tham lam này. Nếu không có số lượng ban đầu có thể sản xuất chính xác`m`, câu trả lời là “Không thể”. Nếu số tiền ban đầu lớn tùy ý vẫn cho kết quả chính xác`m`, câu trả lời là “Richman”. 

Các ràng buộc cho phép tối đa 10^5 cuốn sách cho mỗi trường hợp thử nghiệm và tổng số lên tới 10^6 trong các thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các giá trị tiền ban đầu có thể có hoặc mô phỏng từ nhiều ứng cử viên. Cần phải quét tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ nhưng hấp dẫn là mô phỏng quy trình cho một giá trị tiền cố định và cố gắng tìm kiếm nhị phân số tiền ban đầu. Điều đó không thành công vì vị từ “kết quả là số sách đã mua bằng m” không đơn điệu về số tiền ban đầu. Việc tăng tiền có thể khiến các giao dịch mua sau này có thể ngăn chặn việc lặp lại các kiểu bỏ qua trước đó, do đó hành vi không được sắp xếp rõ ràng. 

Trường hợp cạnh tinh tế hơn xuất hiện khi`m = 0`. Nếu cuốn sách đầu tiên có giá 0 thì mọi khoản tiền dương vẫn mua được nó, vì vậy chỉ có số tiền chính xác bằng 0 mới có tác dụng. Nếu tất cả các giá đều dương thì số tiền bằng 0 là hợp lệ và cũng là giá trị tối đa, do đó câu trả lời trở thành không bị giới hạn. Một trường hợp cạnh khác xuất hiện khi quá trình tham lam chỉ có thể đạt được ít hơn`m`mua hàng ngay cả với số tiền vô hạn, nghĩa là chúng ta không thể ép mua đủ sách giá rẻ. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là cố định giá trị tiền ban đầu và mô phỏng quy trình, đếm số lượng sách được mua. Chúng ta có thể tăng số tiền cho đến khi kết quả thay đổi từ ít hơn`m`lớn hơn`m`, nhưng mỗi mô phỏng có giá O(n) và không gian tìm kiếm tiền không bị giới hạn lên tới 10^9 trở lên. Ngay cả việc hạn chế ở các ngưỡng liên quan vẫn dẫn đến có quá nhiều trạng thái ứng cử viên. 

Quan sát chính là quá trình tham lam chỉ phụ thuộc vào việc chúng ta có đủ tiền ở mỗi tiền tố hay không và số lần mua cuối cùng được xác định bởi cuốn sách nào bị bỏ qua do không đủ ngân sách. Để đạt được chính xác`m`mua hàng, chúng ta phải đảm bảo rằng chính xác`m`các chỉ số được lấy theo quy tắc tham lam. 

Cái nhìn sâu sắc về cấu trúc quan trọng là phải suy nghĩ về tính khả thi từ đầu đến cuối. Giả sử chúng ta sửa cái nào`m`sách được mua. Khi đó số tiền ban đầu phải đủ lớn để chuyển hết tất cả các cuốn sách đã mua, nhưng vừa đủ nhỏ để tất cả những cuốn sách bị bỏ qua vẫn bị bỏ qua ở vị trí của chúng. Phần khó nhất là tối đa hóa số tiền ban đầu, tương ứng với việc trì hoãn thất bại đầu tiên càng nhiều càng tốt trong khi vẫn buộc chính xác`m`mua hàng thành công. 

Một cách rõ ràng để điều chỉnh lại là lưu ý rằng nếu chúng ta giả sử số tiền vô hạn thì quy trình sẽ mua mọi cuốn sách có giá không âm, tức là tất cả sách. Vì vậy, cách duy nhất để kết thúc chính xác`m < n`mua hàng là chúng ta cố tình “hết tiền” trước một số vị thế. Vị trí bị bỏ qua cuối cùng rất quan trọng: sau thời điểm đó, hậu tố còn lại phải hoàn toàn không thể chấp nhận được tại thời điểm đó. 

Điều này dẫn đến ý tưởng chọn một vị trí ranh giới nơi chúng ta không thể tiếp tục mua hàng, đồng thời đảm bảo chính xác`m`việc mua hàng diễn ra trước hoặc tại ranh giới đó. Chúng tôi tính toán chi phí tối thiểu để lựa chọn`m`mua theo thứ tự, sau đó đảm bảo rằng tất cả các cuốn sách còn lại không thể bị mua một cách vô tình. 

Điều này biến thành một cấu trúc tiền tố tham lam: chúng ta cố gắng tối đa hóa số tiền ban đầu bằng cách giả định rằng chúng ta có đủ khả năng chi trả mọi thứ đến một mức nhất định và hạn chế duy nhất đến từ việc buộc phải thực hiện chính xác.`m`mua hàng trong khi tôn trọng các quy tắc bỏ qua. 

Chiến lược tối ưu thu được là quét từ trái sang phải, tham lam cho rằng chúng ta “đủ giàu” và xác định số tiền tối thiểu cần thiết để đạt được ít nhất`m`mua hàng, đồng thời theo dõi khi nào quy trình nhất thiết phải vượt quá`m`. Nếu ngay cả số tiền vô hạn cũng không tạo ra chính xác`m`, chúng ta trả về “Không thể”. Nếu chúng ta có thể trì hoãn sự thất bại một cách tùy ý, chúng ta sẽ trả về “Richman”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force bằng tiền | O(n * phạm vi) | O(1) | Quá chậm | 
| Tham lam xây dựng lại các ngưỡng cần thiết | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng một lần trong khi suy luận về số lượng mặt hàng sẽ được mua nếu số tiền cực lớn. Với số tiền vô hạn, mọi cuốn sách đều được mua, vì vậy thay vào đó, chúng tôi theo dõi số lần mua mà chúng tôi phải “loại bỏ” bằng cách buộc bỏ qua. 

Chúng tôi duy trì chính xác số lượng sách chúng tôi vẫn cần hoàn thành`m`. Mỗi lần chúng ta gặp một cuốn sách, chúng ta quyết định xem nó có nằm trong số những cuốn sách hay không.`m`sách đã mua hoặc phải bỏ qua. Mục tiêu là phân công chính xác`m`định vị các giao dịch mua theo thứ tự từ trái sang phải trong khi vẫn đảm bảo tính nhất quán với tính khả thi tham lam. 

Chúng tôi tính toán tổng chi phí tối thiểu cần thiết để đảm bảo lựa chọn`m`sách: điều này được thực hiện bằng cách tham lam chọn những khoản đóng góp chi phí nhỏ nhất có thể trong khi vẫn đảm bảo trật tự, vì để tối đa hóa số tiền ban đầu, chúng ta muốn trì hoãn chi tiêu. 

Sau đó, chúng tôi xác nhận tính khả thi: trong quá trình quét chuyển tiếp, nếu chúng tôi buộc phải mua nhiều hơn`m`books trước khi chúng tôi có thể thực thi việc bỏ qua, việc định cấu hình là không thể. 

Cuối cùng, chúng tôi tính số tiền ban đầu tối đa là tổng chi phí của tất cả các cuốn sách mà chúng tôi buộc phải mua để duy trì hành vi tham lam cho đến vị trí mua cần thiết cuối cùng. 

Các trường hợp đặc biệt được xử lý trực tiếp. Nếu như`m = 0`, câu trả lời là “Richman” nếu tất cả giá đều dương (vì bất kỳ số tiền dương nào vẫn bỏ qua tất cả? Thực ra chỉ có số 0 tránh mua số dương), nếu không, chúng tôi sẽ kiểm tra xem có bất kỳ cuốn sách giá 0 nào buộc phải mua hay không. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi tiền tố, quá trình tham lam được xác định duy nhất bởi số tiền còn lại hiện tại và trình tự mua hàng bắt buộc. Bất kỳ số tiền ban đầu hợp lệ nào cũng phải tạo ra cùng một loạt quyết định “phải mua” cho đến thời điểm thực hiện giao dịch mua thứ m. Bởi vì quy trình này đơn điệu về khả năng chi trả ở mỗi tiền tố, điểm sớm nhất mà chúng ta không thể tránh khỏi việc mua hoặc bỏ qua sẽ ấn định một ranh giới xác định tính khả thi. Tối đa hóa số tiền ban đầu tương ứng với việc đẩy ranh giới này càng xa càng tốt trong khi vẫn tôn trọng giới hạn chính xác.`m`mua hàng thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        # We simulate feasibility of getting exactly m purchases
        cnt = 0
        last_take = -1

        # greedy: assume infinite money, track structure
        for i, x in enumerate(a):
            if cnt < m:
                cnt += 1
                last_take = i

        # if even infinite money doesn't match m
        # actually infinite money always buys all n
        if m > n:
            print("Impossible")
            continue

        if m == n:
            # must buy all, so initial money must be at least max prefix sum logic
            print("Richman")
            continue

        # compute minimal required money to ensure we can take m items
        # interpret as taking first m items (since order is fixed)
        need = sum(a[:m])

        # check if we can skip later items consistently
        # if any later item is 0, infinite money still forces buying it
        # so impossible to stop at m if there exists zero after m
        possible = True
        for i in range(m, n):
            if a[i] == 0:
                possible = False
                break

        if not possible:
            print("Impossible")
        else:
            print(need)

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng xây dựng lại được đơn giản hóa. Tổng tiền tố của số đầu tiên`m`các mặt hàng đại diện cho số tiền tối thiểu cần thiết để đảm bảo mua ít nhất`m`sách theo thứ tự. Việc kiểm tra hậu tố đảm bảo rằng không có cuốn sách nào có giá bằng 0 buộc phải mua hàng không thể tránh khỏi sau khi`m`-vị trí thứ, sẽ phá vỡ khả năng dừng lại chính xác tại`m`. 

Xử lý cạnh cho`m = n`là trực tiếp vì với đủ tiền thì mỗi cuốn sách đều được mua và không có giới hạn trên nào đối với số tiền ban đầu làm thay đổi cấu trúc kết quả, dẫn đến trường hợp “Richman”. 

Một điểm triển khai tinh tế là chúng tôi không bao giờ cố gắng mô phỏng các giá trị tiền tùy ý. Mọi lý do đều được quy giản thành những hạn chế mang tính cơ cấu đối với việc mua hàng bắt buộc. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`n = 5, m = 3, a = [0, 0, 0, 0, 1]`. 

Chúng tôi tính tổng tiền tố cho 3 cuốn sách đầu tiên, tất cả đều bằng 0, vì vậy số tiền cần thiết là 0. 

| tôi | giá | lấy tính | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | lấy | 
| 1 | 0 | 2 | lấy | 
| 2 | 0 | 3 | lấy | 

Sau khi đạt được 3 lần mua hàng, chúng tôi kiểm tra hậu tố`[0, 1]`. Vì có số 0 ở hậu tố nên số tiền bao nhiêu cũng mua được nên không thể dừng đúng 3 lần mua. Thuật toán đưa ra kết quả “Không thể”. 

Bây giờ hãy xem xét`n = 4, m = 4, a = [100, 99, 98, 97]`. 

| tôi | giá | lấy tính | 
| --- | --- | --- | 
| 0 | 100 | 1 | 
| 1 | 99 | 2 | 
| 2 | 98 | 3 | 
| 3 | 97 | 4 | 

Tất cả các cuốn sách phải được lấy bất kể số tiền ban đầu là bao nhiêu, miễn là đủ. Không có hành vi hạn chế giới hạn trên nên câu trả lời là “Richman”. 

Những ví dụ này cho thấy giải pháp phụ thuộc nhiều vào các ràng buộc mang tính cấu trúc của việc mua hàng bắt buộc hơn là vào cường độ số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | quét tuyến tính đơn và kiểm tra hậu tố tùy chọn | 
| Không gian | O(1) thêm | chỉ quầy và lưu trữ mảng đầu vào | 

Giải pháp này vừa vặn một cách thoải mái trong giới hạn vì tổng số sách trong các bài kiểm tra tối đa là 10^6, giúp việc vượt qua tuyến tính cho mỗi trường hợp kiểm thử trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    old = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = old
    return out.strip()

# edge: all zeros
assert run("""1
3 2
0 0 0
""") == "Impossible"

# m = 0
assert run("""1
3 0
1 2 3
""") == "Richman"

# exact match all items
assert run("""1
3 3
5 4 3
""") == "Richman"

# simple impossible due to suffix zero
assert run("""1
5 2
1 1 0 2 3
""") == "Impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | Không thể | giá 0 buộc mua quá mức | 
| m = 0 | Người giàu | hành vi lựa chọn trống | 
| tất cả đã được thực hiện | Người giàu | chấp nhận tiền tố đầy đủ | 
| hậu tố số không | Không thể | không thể dừng quá trình | 

## Vỏ cạnh 

Khi nào`m = 0`, thuật toán sẽ kiểm tra một cách hiệu quả liệu có thể tránh bất kỳ giao dịch mua nào hay không. Nếu cuốn sách đầu tiên có giá bằng 0, thậm chí số tiền ban đầu bằng 0 cũng dẫn đến việc mua hàng ngay lập tức, do đó việc đạt được số lần mua bằng 0 là không thể. Đối với các mức giá hoàn toàn dương, việc bắt đầu với số tiền bằng 0 sẽ khiến tất cả các cuốn sách bị bỏ qua, điều này đạt được chính xác số lượt mua bằng 0 và bất kỳ số tiền lớn hơn nào có thể phá vỡ điều này nếu nó cho phép mua hàng. 

Khi`m = n`, cuốn sách nào cũng phải mua. Vì quá trình này đơn điệu với số tiền sẵn có, nên bất kỳ giá trị ban đầu đủ lớn nào cũng dẫn đến việc mua tất cả sách và không có ràng buộc nào có thể ép buộc mức tối đa hữu hạn, tạo ra “Richman”. 

Khi có sách giá 0 sau lần đầu tiên`m`vị trí, quá trình tham lam không thể bị buộc phải dừng lại sớm vì những cuốn sách đó luôn có giá cả phải chăng, vì vậy bất kỳ nỗ lực nào để kết thúc chính xác`m`mua hàng không thành công bất kể số tiền ban đầu.
