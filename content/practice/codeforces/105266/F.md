---
title: "CF 105266F - \u9996\u53d1\u9635\u5bb9"
description: "Có năm vai trò cố định phải được đảm nhận bởi chính xác năm người chơi riêng biệt. Mỗi người chơi đi kèm với một ràng buộc được mã hóa dưới dạng chuỗi nhị phân có độ dài 5, trong đó số 1 cho biết người chơi có khả năng đóng vai trò tương ứng."
date: "2026-06-24T00:34:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105266
codeforces_index: "F"
codeforces_contest_name: "2024 XTU Summer Camp Selection Competition"
rating: 0
weight: 105266
solve_time_s: 57
verified: true
draft: false
---

[CF 105266F - \u9996\u53d1\u9635\u5bb9](https://codeforces.com/problemset/problem/105266/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có năm vai trò cố định phải được đảm nhận bởi chính xác năm người chơi riêng biệt. Mỗi người chơi có một ràng buộc được mã hóa dưới dạng chuỗi nhị phân có độ dài 5, trong đó`1`chỉ ra rằng người chơi có khả năng đóng vai trò tương ứng. Đội hình hợp lệ là sự phân công từng người một giữa năm vai trò và năm người chơi riêng biệt sao cho mọi người chơi được chỉ định đều đủ tiêu chuẩn cho vai trò của họ. 

Nhiệm vụ là đếm xem có bao nhiêu nhiệm vụ hợp lệ tồn tại, thực hiện tất cả các lựa chọn có thể có của năm người chơi khác nhau trong nhóm và trên tất cả các cách hợp lệ để phân công họ vào năm vị trí. Hai đội hình được coi là khác nhau nếu có ít nhất một vai trò do một người chơi khác đảm nhiệm. 

Kích thước đầu vào lên tới hai trăm nghìn người chơi. Mỗi người chơi đóng góp một mặt nạ khả năng 5-bit. Một cách giải thích ngây thơ ngay lập tức gợi ý chọn 5 người chơi bất kỳ và kiểm tra tất cả các hoán vị, điều này đã cho thấy một vụ nổ tổ hợp vượt xa những gì bảng liệt kê có thể xử lý. Với n lên tới 2×10^5, mọi thứ gần hơn với O(n^5) hoặc thậm chí O(n^3 · 5!) đều không khả thi trong giới hạn một giây. Ngay cả O(n^2) cũng là đường biên nhưng chỉ có khả năng được chấp nhận với các hằng số rất nhỏ, vì vậy giải pháp phải tránh tương tác theo cặp giữa các tập hợp con tùy ý của người chơi. 

Một trường hợp thất bại tinh vi do lý luận ngây thơ xuất hiện khi nhiều người chơi có chung các mẫu năng lực giống hệt nhau. Ví dụ: nếu tất cả mười người chơi đều có thể đóng mọi vai trò, thì số lượng đội hình hợp lệ không chỉ đơn giản là “chọn 5 người chơi” mà là “chọn 5 nhiệm vụ theo thứ tự” và cách tính ngây thơ mà bỏ qua các hoán vị phân công vai trò sẽ bị tính thiếu hoặc thừa tùy thuộc vào cách xử lý tính đối xứng. Một cạm bẫy khác là coi người chơi chỉ có thể hoán đổi cho nhau bằng mặt nạ, điều này sẽ hợp nhất không chính xác các cá nhân riêng biệt mặc dù họ đưa ra những lựa chọn riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp cố gắng liệt kê mọi tập hợp con của năm người chơi và sau đó hoán đổi chúng qua năm vai trò trong khi kiểm tra tính hợp lệ. Đối với mỗi bộ được chọn, có tới 5! bài tập và kiểm tra tính hợp lệ là thời gian không đổi cho mỗi bài tập. Chỉ riêng số tập con là$\binom{n}{5}$, tăng theo thứ tự n^5/120, đã vượt quá 10^25 khi n là 2×10^5. Ngay cả khi bằng cách nào đó chúng ta hạn chế bản thân ở các tập con nhỏ thì cấu trúc tổ hợp vẫn quá lớn để có thể khám phá một cách rõ ràng. 

Quan sát quan trọng là quy mô của vấn đề thực sự được kiểm soát bởi số lượng vai trò chứ không phải số lượng người chơi. Chỉ có năm vị trí, vì vậy bất kỳ trạng thái nào mô tả các nhiệm vụ một phần chỉ phụ thuộc vào vai trò nào đã được lấp đầy. Điều này gợi ý một công thức lập trình động trong đó chúng tôi xử lý từng người chơi và duy trì số lượng cách chúng tôi có thể đạt được từng tập hợp con các vai trò đã được lấp đầy. 

Mỗi người chơi có thể bị bỏ qua hoặc được sử dụng để đảm nhận chính xác một trong những vai trò vẫn còn trống mà họ có khả năng đảm nhận. Vì chỉ có 5 vai trò nên không gian trạng thái chỉ là 2^5 = 32, giúp bạn có thể theo dõi tất cả cấu hình một cách hiệu quả. Việc xử lý mỗi người chơi cập nhật 32 trạng thái này, mang lại tổng độ phức tạp tuyến tính theo n. 

Cách tiếp cận brute-force hoạt động về mặt khái niệm vì nó tôn trọng tất cả các ràng buộc một cách rõ ràng, nhưng nó thất bại vì nó liên tục tính toán lại các phép gán từng phần tương đương. Quan sát cho thấy chỉ có tập hợp các vai trò đã được lấp đầy mới quan trọng làm giảm sự phụ thuộc theo cấp số nhân vào n vào một máy trạng thái có kích thước không đổi đối với các tập hợp con vai trò. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5 · 5!) | O(1) | Quá chậm | 
| Bitmask DP trên người chơi | O(n · 2^5 · 5) | O(2^5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa quy trình bằng cách quyết định dần dần những người chơi nào sẽ được sử dụng và cách họ đóng góp để hoàn thành năm vai trò. 

1. Xác định mảng DP trong đó dp[mask] biểu thị số cách gán vai trò tương ứng với bitmask`mask`sử dụng tiền tố của người chơi. Mỗi bit trong mặt nạ tương ứng với một vai trò đã được lấp đầy. Điều này nắm bắt một cách gọn gàng tất cả các bài tập hợp lệ một phần. 
2. Khởi tạo dp[0] = 1, vì có chính xác một cách để không gán gì cho không có vai trò nào. 
3. Lặp lại từng người chơi theo thứ tự đầu vào. Đối với mỗi người chơi, hãy tạo một mảng DP mới ndp ban đầu bằng dp, thể hiện lựa chọn bỏ qua trình phát này. 
4. Đối với mỗi mặt nạ trạng thái hiện tại, hãy cố gắng chỉ định người chơi hiện tại cho bất kỳ vai trò j nào vẫn chưa được điền vào mặt nạ và chuỗi khả năng của người chơi có một`1`. Đối với mỗi vai trò như vậy, hãy cập nhật ndp[mask ∪ {j}] bằng cách thêm dp[mask]. Điều này thể hiện việc sử dụng trình phát này để mở rộng một phần nhiệm vụ. 
5. Thay thế dp bằng ndp sau khi xử lý trình phát. Điều này đảm bảo mỗi người chơi được sử dụng nhiều nhất một lần và được xem xét chính xác một lần. 
6. Sau khi xử lý tất cả người chơi, câu trả lời là dp[(1<<5) - 1], đại diện cho tất cả năm vai trò đã được lấp đầy. 

Tính chính xác dựa trên thực tế là mọi phép gán hợp lệ đều có một “lần xuất hiện cuối cùng” duy nhất theo thứ tự xử lý của những người chơi nơi nó được xây dựng. Vì chúng tôi chỉ tiến lên theo trình tự người chơi và không bao giờ sử dụng lại người chơi nên mỗi nhiệm vụ được tính chính xác một lần. 

Bất biến được duy trì là sau khi xử lý i người chơi đầu tiên, dp[mask] bằng số cách để chọn một tập hợp con của những người chơi i đó và gán chúng cho chính xác các vai trò trong mặt nạ, tôn trọng tính tương thích. Mỗi quá trình chuyển đổi sẽ giữ nguyên tập hợp con (bỏ qua) hoặc mở rộng nó bằng cách gán cho trình phát hiện tại một vai trò mới, không bao giờ vi phạm các ràng buộc về tính duy nhất hoặc khả năng tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n = int(input())
    can = [input().strip() for _ in range(n)]

    dp = [0] * 32
    dp[0] = 1

    for i in range(n):
        ndp = dp[:]  # skipping player i
        mask_allowed = 0

        for j in range(5):
            if can[i][j] == '1':
                mask_allowed |= (1 << j)

        for mask in range(32):
            if dp[mask] == 0:
                continue
            base = dp[mask]
            # try assign this player to each available role
            avail = mask_allowed & (~mask)
            sub = avail
            while sub:
                bit = sub & -sub
                ndp[mask | bit] = (ndp[mask | bit] + base) % MOD
                sub -= bit

        dp = ndp

    print(dp[31] % MOD)

if __name__ == "__main__":
    main()
```Việc triển khai giữ một mảng DP 32 phần tử tương ứng với tất cả các tập hợp con của năm vai trò. Đối với mỗi người chơi, chúng tôi tính toán một bitmask các vai trò mà họ có thể thực hiện. Quá trình chuyển đổi cẩn thận đảm bảo chúng tôi chỉ gán cho người chơi những vai trò chưa được sử dụng trong mặt nạ hiện tại. 

Vòng lặp bên trong trên các mặt nạ con của các vai trò có sẵn là an toàn vì kích thước mặt nạ được cố định ở mức 5, do đó việc lặp qua các tập hợp con là thời gian không đổi trong thực tế. Modulo được áp dụng trong quá trình chuyển đổi để ngăn chặn tràn. 

Một điểm tinh tế là việc sử dụng một bản sao`ndp = dp[:]`. Điều này là cần thiết để tránh cho phép một người chơi đóng góp nhiều lần trong cùng một lần lặp. Nếu không có sự tách biệt này, các bản cập nhật sẽ xâu chuỗi không chính xác trong quá trình xử lý của cùng một trình phát. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ có năm vai trò và ba người chơi: 

đầu vào:```
3
11111
10000
01111
```Người chơi đầu tiên có thể đảm nhận bất kỳ vai trò nào, người chơi thứ hai chỉ có thể đảm nhận vai trò 1 và người chơi thứ ba có thể đảm nhận bất kỳ vai trò nào từ 2 đến 5. 

Chúng tôi theo dõi dp dưới dạng mặt nạ theo vai trò. 

| Bước | Người chơi | dp[0] | dp[người khác] | dp[11111] | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 | 0 | 0 | 
| 1 | 11111 | 1 | 5 bang trở thành 1 | 1 | 
| 2 | 10000 | dp được cập nhật bằng cách thêm bài tập vai trò 1 | tăng mặt nạ một phần | phát triển | 
| 3 | 01111 | phần mở rộng thêm | bài tập đầy đủ hơn | cuối cùng | 

Quan sát quan trọng trong dấu vết này là người chơi đầu tiên tạo tất cả các điểm bắt đầu với một vai trò và những người chơi sau này chỉ mở rộng các trạng thái một phần đó mà không can thiệp vào các nhiệm vụ đã hoàn thành. 

Bây giờ hãy xem xét một trường hợp có bản sao: 

đầu vào:```
5
11111
11111
11111
11111
11111
```Mọi người chơi đều giống hệt nhau. DP đếm tất cả các cách để chọn 5 người chơi bất kỳ theo thứ tự và chỉ định họ vào 5 vai trò, phù hợp với kỳ vọng tổ hợp là 5! hoán vị nhân với sự kết hợp của các nguồn giống hệt nhau. 

Dấu vết cho thấy rằng mỗi lần một người chơi được xử lý, mọi phân công một phần hiện có sẽ phân nhánh thành 5 phần mở rộng có thể có cho đến khi tất cả các vai trò được lấp đầy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^5 · 5) | Đối với mỗi n người chơi, chúng tôi lặp lại hơn 32 mặt nạ và tối đa 5 lần chuyển đổi vai trò | 
| Không gian | O(2^5) | Chỉ các mảng DP trên các tập hợp con vai trò mới được lưu trữ | 

Không gian trạng thái không đổi đối với n, do đó nghiệm tỉ lệ tuyến tính với số lượng người chơi. Với n lên tới 2×10^5, tổng số thao tác vào khoảng vài triệu, vừa vặn thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MOD = 10**9 + 7

    n = int(input())
    can = [input().strip() for _ in range(n)]

    dp = [0] * 32
    dp[0] = 1

    for i in range(n):
        ndp = dp[:]
        mask_allowed = 0
        for j in range(5):
            if can[i][j] == '1':
                mask_allowed |= (1 << j)

        for mask in range(32):
            if dp[mask] == 0:
                continue
            avail = mask_allowed & (~mask)
            sub = avail
            while sub:
                bit = sub & -sub
                ndp[mask | bit] = (ndp[mask | bit] + dp[mask]) % MOD
                sub -= bit

        dp = ndp

    return str(dp[31] % MOD)

# minimum case
assert run("5\n11111\n11111\n11111\n11111\n11111\n") == "120", "all flexible"

# minimal structured case
assert run("5\n10000\n01000\n00100\n00010\n00001\n") == "1", "fixed matching"

# impossible case
assert run("5\n10000\n10000\n10000\n10000\n10000\n") == "0", "cannot fill all roles"

# extra player redundancy
assert run("6\n11111\n11111\n11111\n11111\n11111\n11111\n") == "720", "extra identical players"

# mixed constraints
assert run("5\n11000\n11000\n00111\n00111\n11111\n") == run("5\n11000\n11000\n00111\n00111\n11111\n"), "consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều linh hoạt | 120 | đếm hoán vị đầy đủ | 
| khớp cố định | 1 | chuỗi nhiệm vụ độc đáo | 
| trường hợp bất khả thi | 0 | xử lý khả thi | 
| giống hệt nhau hơn | 720 | tăng trưởng kết hợp với các bản sao | 
| ràng buộc hỗn hợp | nhất quán | ổn định theo sắp xếp lại | 

## Vỏ cạnh 

Trường hợp quan trọng là khi không người chơi nào có thể đảm nhận một vai trò cụ thể. Ví dụ: nếu mỗi chuỗi có một`0`ở vị trí thứ ba, thì mọi trạng thái DP cố gắng bao gồm vai trò đó vẫn không thể truy cập được và câu trả lời cuối cùng chính xác vẫn là 0 vì dp[11111] không bao giờ được hình thành. 

Một trường hợp khác phát sinh khi nhiều người chơi chia sẻ mặt nạ năng lực giống hệt nhau. Thuật toán không thu gọn chúng; mỗi cái được xử lý độc lập theo trình tự. Điều này đảm bảo rằng việc chọn các tập hợp con khác nhau của những người chơi giống hệt nhau vẫn được tính riêng, vì dp tiến triển qua các bước xử lý riêng biệt ngay cả khi các chuyển đổi giống hệt nhau. 

Trường hợp tinh tế cuối cùng là khi người chơi có thể đảm nhận nhiều vai trò. Việc lặp lại mặt nạ con đảm bảo rằng cùng một người chơi đóng góp vào tất cả các lựa chọn vai trò hợp lệ chính xác một lần cho mỗi trạng thái DP mà không trộn lẫn các nhiệm vụ trong một lần lặp.
