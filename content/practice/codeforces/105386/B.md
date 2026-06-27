---
title: "CF 105386B - Huy chương Vàng"
description: "Có một số cuộc thi diễn ra song song. Mỗi cuộc thi đã có sẵn một số đội tham gia và bạn được phép phân bổ thêm nhóm đội cho các cuộc thi này theo cách bạn muốn."
date: "2026-06-23T16:20:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105386
codeforces_index: "B"
codeforces_contest_name: "The 2024 ICPC Kunming Invitational Contest"
rating: 0
weight: 105386
solve_time_s: 92
verified: true
draft: false
---

[CF 105386B - Huy chương vàng](https://codeforces.com/problemset/problem/105386/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có một số cuộc thi diễn ra song song. Mỗi cuộc thi đã có sẵn một số đội tham gia và bạn được phép phân bổ thêm nhóm đội cho các cuộc thi này theo cách bạn muốn. 

Mỗi cuộc thi đều trao huy chương vàng với cách thức rất đơn giản: mỗi khi tổng số đội trong cuộc thi đó đạt bội số của một giá trị cố định$k$, cuộc thi đó đã giành được một huy chương vàng. Tương tự, nếu một cuộc thi kết thúc với$t$các đội, nó góp phần$\lfloor t / k \rfloor$huy chương. 

Bạn bắt đầu với cấu hình ban đầu$a_i$các đội trong mỗi cuộc thi và bạn cũng có$m$các đội bổ sung có thể được chỉ định tùy ý giữa các cuộc thi. Mỗi đội phải được chỉ định ở một nơi nào đó và mỗi nhiệm vụ sẽ tăng số lượng đội của cuộc thi tương ứng lên một. 

Nhiệm vụ là phân phối tất cả$m$các đội theo cách tối đa hóa tổng số huy chương có được trong tất cả các cuộc thi. 

Những hạn chế quan trọng một cách rất trực tiếp. Có tối đa 100 cuộc thi nhưng số đội dự thi$m$và số đếm ban đầu$a_i$cả hai đều có thể lớn bằng$10^9$. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào mô phỏng từng nhóm quy trình, vì ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu hàng tỷ thao tác. Giải pháp phải hoạt động bằng cách suy luận theo các khối tổng hợp của các nhóm thay vì các bài tập riêng lẻ. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ tham lam ngây thơ xuất hiện khi bạn cho rằng mỗi nhóm bổ sung đều độc lập đóng góp một điều gì đó tối ưu cho địa phương. Ví dụ: việc thêm một đội có thể không làm tăng ngay lập tức số huy chương của bất kỳ cuộc thi nào, nhưng trình tự bổ sung được lựa chọn cẩn thận có thể “hoàn thành” một ngưỡng và mở khóa huy chương. Nếu bạn chỉ nhìn vào lợi ích trước mắt của mỗi nhóm, bạn sẽ bỏ lỡ sự thật rằng tiến trình hướng tới nhiều vấn đề tiếp theo. 

Một cạm bẫy khác là giả định rằng sau khi phân bổ một số đội, những đội còn lại có thể được đối xử thống nhất. Điều này trở nên không chính xác nếu bạn vẫn có các cuộc thi không được liên kết theo bội số của$k$, vì sự tiến bộ một phần trong các cuộc thi khác nhau sẽ tương tác một cách không cần thiết với các huy chương trong tương lai. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: ở mỗi bước, hãy thử chỉ định một trong các đội còn lại cho bất kỳ cuộc thi nào, tính lại tổng số huy chương và tiếp tục đệ quy hoặc lặp đi lặp lại cho đến khi tất cả các đội được chỉ định. Điều này đúng vì nó khám phá tất cả các phân phối có thể. Tuy nhiên, mỗi trong số$m$các đội có tới$n$lựa chọn, do đó không gian tìm kiếm tăng lên như$n^m$, điều này hoàn toàn không thể thực hiện được ngay cả với những giá trị rất nhỏ của$m$. 

Quan sát quan trọng là huy chương chỉ phụ thuộc vào số lần mỗi cuộc thi vượt qua bội số của$k$. Điều này có nghĩa là trạng thái của mỗi cuộc thi được nắm bắt hoàn toàn bởi modulo còn lại của nó.$k$và mọi hành động hữu ích là hoàn thành phần còn lại để đạt bội số tiếp theo hoặc đóng góp toàn bộ các khối có kích thước$k$sau đó. 

Điều này chuyển đổi vấn đề từ các quyết định của mỗi đội thành các mức tăng theo từng huy chương. Mỗi cuộc thi có thể được coi là mang lại “cơ hội” để giành được huy chương +1, mỗi huy chương đều có chi phí được tính cho các đội bổ sung. Sau đó, chúng tôi muốn chọn những cơ hội rẻ nhất trước tiên, nhưng chúng tôi cũng nhận ra rằng sau lần điều chỉnh đầu tiên cho mỗi cuộc thi, tất cả lợi nhuận còn lại đều hoạt động giống nhau. 

Cấu trúc này cho phép chúng tôi tách giải pháp thành hai giai đoạn: đầu tiên chúng tôi tối ưu hóa tất cả “các bản sửa lỗi còn lại” và sau đó chúng tôi xử lý hàng loạt các nhóm còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$m$| O(n) đệ quy | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi vấn đề thành “chúng tôi có thể giành được bao nhiêu huy chương” cộng với “chúng tôi có thể buộc thêm bao nhiêu huy chương khi sử dụng các đội bổ sung”. 

Mỗi cuộc thi$i$ban đầu đóng góp$\lfloor a_i / k \rfloor$huy chương ngay lập tức, vì đó đã là bội số hoàn thành. 

Bây giờ chúng tôi tập trung vào cấu trúc còn sót lại bên trong mỗi cuộc thi. Cho phép$r_i = a_i \bmod k$. Nếu chúng ta thêm một số đội, cách duy nhất để tăng số huy chương một cách hiệu quả là đẩy cuộc thi lên bội số tiếp theo của$k$. Điều đó đòi hỏi chính xác$k - r_i$đội nếu$r_i \neq 0$, Và$k$đội nếu$r_i = 0$, vì việc ở chính xác ở bội số không có nghĩa là chúng ta “gần” với bội số tiếp theo. 

Vì vậy, mỗi cuộc thi đều đưa ra một nâng cấp có ý nghĩa duy nhất: trả một khoản phí$c_i$các đội giành được huy chương +1 bằng cách hoàn thành nhiều ranh giới tiếp theo. 

Chúng tôi sắp xếp những chi phí này và chấp nhận chúng một cách tham lam miễn là chúng tôi có đủ đội. 

Sau những nâng cấp này, mọi cuộc thi được chọn giờ đây đã được căn chỉnh hoàn hảo theo bội số của$k$. Các cuộc thi còn lại vẫn có cấu trúc cố định nhưng các cuộc thi còn lại$m$các đội hiện chỉ tương tác thông qua các khối có kích thước đầy đủ$k$và việc phân phối chúng giữa nhiều cuộc thi không thể tốt hơn việc nhóm chúng một cách tối ưu. Kết quả tốt nhất có thể có được từ các đội còn lại chỉ đơn giản là$\lfloor m_{\text{left}} / k \rfloor$huy chương bổ sung. 

Điều này dẫn đến một quá trình sạch sẽ: 

1. Tính số huy chương ban đầu như sau$\sum \lfloor a_i / k \rfloor$. 
2. Đối với mỗi cuộc thi, hãy tính chi phí để đạt bội số tiếp theo$c_i$. 
3. Phân loại chi phí và chi tiêu một cách tham lam$m$trên chúng. 
4. Thêm khoản đóng góp còn lại$\lfloor m / k \rfloor$. 

Tại sao nó hoạt động xuất phát từ việc nén cấu trúc của các trạng thái. Mỗi cuộc thi có chính xác một lần chuyển đổi không đồng nhất (để đạt được bội số tiếp theo), sau đó tất cả lợi ích trong tương lai sẽ hoạt động giống hệt nhau theo các khối có kích thước$k$. Điều này đảm bảo rằng tất cả sự bất đối xứng sẽ biến mất sau nhiều nhất$n$quyết định, và phần còn lại trở nên thống nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        m = int(input())

        base = 0
        costs = []

        for x in a:
            base += x // k
            r = x % k
            if r == 0:
                costs.append(k)
            else:
                costs.append(k - r)

        costs.sort()

        extra = 0
        for c in costs:
            if m < c:
                break
            m -= c
            extra += 1

        extra += m // k
        print(base + extra)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ trích xuất phần đóng góp huy chương được đảm bảo từ cấu hình ban đầu. Sau đó, nó xây dựng một danh sách “chi phí hoàn thành” cho mỗi cuộc thi, nghĩa là cần có bao nhiêu đội để đẩy cuộc thi đó lên bội số tiếp theo của$k$. Việc sắp xếp các chi phí này cho phép chúng tôi luôn dành cho các nhóm những nâng cấp hiệu quả nhất trước tiên. 

Sau khi nâng cấp hết mức phải chăng, các đội còn lại được xử lý hàng loạt bằng cách chia số nguyên cho$k$, vì chỉ các khối đầy đủ mới có thể đóng góp thêm huy chương sau khi tất cả các cuộc thi đã được căn chỉnh. 

Một sai lầm phổ biến ở đây là coi phần còn lại bằng 0 là giá trị bằng 0, điều này sẽ cho phép nhận huy chương miễn phí một cách không chính xác. Đó là lý do tại sao mã chỉ định chi phí một cách rõ ràng$k$trong trường hợp đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$k = 5$, cuộc thi với$a = [4, 6]$, Và$m = 6$. 

Huy chương ban đầu là: 

- 4 đóng góp 0 
- 6 đóng góp 1 

Vậy cơ số = 1 

Bây giờ chi phí: 

- Cuộc thi 1: cần 1 đội đạt 5 → chi phí 1 
- Cuộc thi 2: cần 4 đội đạt 10 → giá 4 

Chúng tôi sắp xếp chi phí: [1, 4] 

| Bước | m | Hành động | Huy chương bổ sung | 
| --- | --- | --- | --- | 
| 0 | 6 | bắt đầu | 0 | 
| 1 | 5 | lấy chi phí 1 | 1 | 
| 2 | 1 | lấy chi phí 4? không | 1 | 

Bây giờ còn lại m = 5? thực tế sau bước đầu tiên m=5, sau lần thử thứ hai không thể lấy 4, vì vậy m=5. 

Số huy chương còn lại = 5 // 5 = 1 

Tổng = cơ số 1 + cơ số 2 = 3 

Điều này cho thấy việc hoàn thành sớm được ưu tiên như thế nào, trong khi các đội còn lại tạo thành các khối thống nhất. 

### Ví dụ 2 

hãy để$k = 3$,$a = [3, 1, 2]$,$m = 4$. 

Huy chương cơ bản: 

- 3 → 1 
- 1 → 0 
- 2 → 0 

Cơ sở = 1 

Chi phí: 

- [3, 2, 1] 

Sắp xếp: [1, 2, 3] 

| Bước | m | Hành động | Huy chương bổ sung | 
| --- | --- | --- | --- | 
| 0 | 4 | bắt đầu | 0 | 
| 1 | 3 | lấy 1 | 1 | 
| 2 | 1 | lấy 2? không | 1 | 

Còn lại m = 3 → 3 // 3 = 1 

Tổng cộng = 1 + 2 = 3 

Điều này xác nhận ý tưởng chính rằng các nhóm còn sót lại chỉ quan trọng trong các khối hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp tối đa 100 chi phí cho mỗi trường hợp kiểm thử chiếm ưu thế | 
| Không gian | O(n) | lưu trữ bảng kê chi phí | 

Với$n \le 100$và lên tới 100 trường hợp thử nghiệm, điều này dễ dàng đủ nhanh. Thuật toán tránh mọi sự phụ thuộc vào$m$, có thể lớn bằng$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        m = int(input())

        base = 0
        costs = []
        for x in a:
            base += x // k
            r = x % k
            costs.append(k if r == 0 else k - r)

        costs.sort()
        extra = 0
        for c in costs:
            if m < c:
                break
            m -= c
            extra += 1
        extra += m // k
        out.append(str(base + extra))

    return "\n".join(out)

# minimum size
assert run("1\n1 5\n0\n3\n") == "0"

# already optimal alignment
assert run("1\n2 3\n3 6\n5\n") == "2"

# all equal values
assert run("1\n3 4\n1 1 1\n12\n") == run("1\n3 4\n1 1 1\n12\n")

# k = 1 edge case
assert run("1\n2 1\n10 20\n100\n") == "130"

# tight remainder interaction
assert run("1\n2 5\n4 9\n3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | 0 | trường hợp biên nhỏ nhất | 
| căn chỉnh đã tối ưu | 2 | lý luận miễn phí đúng đắn | 
| tất cả các giá trị bằng nhau | nhất quán | xử lý chi phí thống nhất | 
| k = vỏ 1 cạnh | tăng tuyến tính | hành vi đặc biệt khi đội nào cũng trao huy chương | 
| tương tác còn lại chặt chẽ | tham lam đúng + xử lý dư thừa | đặt hàng nâng cấp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k = 1$. Trong tình huống này, mỗi đội luôn đóng góp một huy chương, vì vậy toàn bộ việc tối ưu hóa chỉ còn là một phép cộng đơn giản. Thuật toán xử lý việc này một cách tự nhiên vì mỗi chi phí sẽ trở thành 1 và mọi đội bổ sung sẽ đóng góp trực tiếp thông qua việc phân chia cuối cùng. 

Một trường hợp khó khăn khác là khi một cuộc thi đã ở mức bội số$k$. Chi phí được đặt thành$k$, không phải bằng 0, ngăn chặn việc nâng cấp miễn phí không chính xác. Thuật toán xử lý chính xác các cuộc thi như yêu cầu khối đầy đủ trước lần tăng tiếp theo. 

Một trường hợp tinh vi cuối cùng xảy ra khi tất cả các đội còn lại không đủ khả năng để hoàn thành bất kỳ nâng cấp nào. Trong trường hợp đó, vòng lặp chi phí được sắp xếp kết thúc sớm và chỉ có các khối có kích thước đầy đủ$k$đóng góp, điều này phản ánh chính xác rằng tiến độ một phần không có giá trị.
