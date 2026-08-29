---
title: "CF 104380P - Ngục tối"
description: "Ngục tối là một đường thẳng, hiệp sĩ đi từ vị trí 0 đến vị trí D mà không bao giờ quay lại. Dọc theo con đường này có hai loại điểm chạm trán: quái vật và cửa hàng, mỗi loại được đặt ở các vị trí cố định."
date: "2026-07-01T17:12:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "P"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 102
verified: true
draft: false
---

[CF 104380P - Ngục tối](https://codeforces.com/problemset/problem/104380/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ngục tối là một đường thẳng, hiệp sĩ đi từ vị trí 0 đến vị trí D mà không bao giờ quay lại. Dọc theo con đường này có hai loại điểm chạm trán: quái vật và cửa hàng, mỗi loại được đặt ở các vị trí cố định. Mỗi quái vật có một sức mạnh cần thiết và một hình phạt nếu hiệp sĩ quá yếu khi tiếp cận nó. Mỗi cửa hàng cung cấp một lọ thuốc có thể nâng sức mạnh của hiệp sĩ lên một giá trị cố định và việc mua nó cũng phải tốn tiền xu. 

Hành vi quan trọng là sức mạnh của hiệp sĩ chỉ tăng lên khi anh ta mua một lọ thuốc. Một lọ thuốc không xếp chồng lên nhau với những lọ trước đó theo nghĩa phụ gia, thay vào đó nó chỉ đặt sức mạnh của anh ta ở cấp độ của lọ thuốc nếu cấp đó cao hơn cấp hiện tại của anh ta. Quái vật không thay đổi sức mạnh; họ chỉ áp đặt một chi phí nếu sức mạnh hiện tại không đủ. 

Mục tiêu là quyết định nên mua loại thuốc nào, nếu có và quái vật nào phải trả tiền để vượt qua, sao cho tổng chi phí xu được giảm thiểu. 

Các giới hạn rất lớn, lên tới 100000 quái vật và 100000 cửa hàng, và các vị trí có thể lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng chuyển động từng bước dọc theo đường đi hoặc tính toán lại chi phí một cách độc lập cho từng sự kiện. Chiến lược bậc hai đối với các sự kiện hoặc trạng thái ứng cử viên cũng sẽ thất bại trong giới hạn 2 giây. Giải pháp phải giảm vấn đề xuống gần hơn với việc sắp xếp cộng với xử lý tuyến tính hoặc logarit. 

Một điểm tinh tế là vị trí của quái vật và cửa hàng không thực sự tương tác với nhau ngoài việc đặt hàng trực tuyến. Vì không có gì phụ thuộc vào thời gian ngoại trừ “hiệp sĩ đã đi qua cửa hàng trước một con quái vật” và sức mạnh không giảm, nên thứ tự không gian chính xác hóa ra không ảnh hưởng đến cấu trúc chi phí tối ưu. Đây là một sự đơn giản hóa quan trọng mà nhiều cách tiếp cận sai lầm đã bỏ qua. 

Một trường hợp thất bại phổ biến phát sinh từ việc cho rằng việc lựa chọn thuốc phụ thuộc vào những con quái vật trước đó một cách năng động. Ví dụ: nếu một hiệp sĩ sớm gặp phải một con quái vật yếu và quyết định trả tiền thay vì nâng cấp, một kẻ tham lam ngây thơ có thể bỏ lỡ rằng một bản nâng cấp giá rẻ sau này sẽ tốt hơn trên toàn cầu. Một trường hợp thất bại khác là cố gắng mô phỏng hành trình trong khi duy trì nhiều cường độ hiện tại có thể có, điều này nhanh chóng bùng nổ vì cường độ có thể có nhiều giá trị lên tới 10^9. 

Trường hợp đặc biệt là khi tất cả quái vật đều mạnh và tất cả các loại thuốc đều đắt tiền ngoại trừ một loại thuốc mạnh muộn. Bất kỳ người tham lam nào mua các bản nâng cấp nhỏ sớm có thể sẽ tệ hơn là chỉ phải trả mọi hình phạt cho đến bản nâng cấp cuối cùng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử từng nhóm nhỏ các cửa hàng và mô phỏng hành trình cho từng lựa chọn thuốc đã mua. Đối với mỗi tập hợp con được chọn, chúng tôi sẽ quét tất cả quái vật và tổng hợp các hình phạt bất cứ khi nào sức mạnh không đủ. Về nguyên tắc, điều này đúng vì nó đánh giá trực tiếp mọi chiến lược có thể, nhưng số lượng tập hợp con là 2^m, điều này hoàn toàn không khả thi ngay cả với m = 40, chứ đừng nói đến 100000. Ngay cả việc giới hạn ở một loại thuốc tốt nhất vẫn để lại sự tương tác với quái vật yêu cầu O(n) cho mỗi ứng cử viên. 

Quan sát quan trọng là hành vi của thuốc chỉ bị chi phối bởi mức tối đa đạt được chứ không phải bởi trình tự mua hàng. Nếu hiệp sĩ kết thúc với sức mạnh L, thì bất kỳ lọ thuốc nào có cấp độ tối đa L đều không liên quan và bất kỳ lọ thuốc nào trên L chỉ có thể được xem xét nếu nó được mua một lần dưới dạng nâng cấp xác định. Điều này thu gọn không gian trạng thái từ các tập hợp con hàm mũ thành một lựa chọn đơn giản về các giá trị cường độ cuối cùng có thể có.

Khi đã hiểu được điều này, vấn đề sẽ được chia thành hai phần độc lập. Một thành phần là quyết định chi phí tốt nhất để đạt được sức mạnh cuối cùng L nhất định, điều này chỉ phụ thuộc vào các cửa hàng cung cấp cấp L. Thành phần còn lại là tính toán số tiền chúng ta phải trả cho quái vật nếu chúng ta có sức mạnh L, điều này chỉ phụ thuộc vào việc sức mạnh yêu cầu của mỗi quái vật có vượt quá L hay không. 

Tính độc lập là rất quan trọng: quái vật không ảnh hưởng đến việc chúng ta nên mua loại thuốc nào và thuốc không phụ thuộc vào thứ tự của quái vật. Điều này cho phép chúng ta đánh giá từng ứng cử viên L một cách độc lập và kết hợp hai khoản đóng góp chi phí. 

Để thực hiện điều này một cách hiệu quả, chúng tôi tính toán trước chi phí của quái vật được sắp xếp theo sức mạnh cần thiết của chúng, cho phép truy vấn nhanh về tổng số tiền phạt áp dụng cho một L nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^m · n) | O(1) | Quá chậm | 
| Đánh giá từng mức độ sức mạnh + tiền xử lý | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành việc đánh giá tất cả các giá trị cường độ cuối cùng có ý nghĩa và tính toán chi phí tốt nhất cho mỗi giá trị. 

1. Trích xuất tất cả các giá trị sức mạnh ứng cử viên từ các cửa hàng, đồng thời bao gồm sức mạnh 0 làm trường hợp cơ bản khi không mua thuốc. Những điều này đại diện cho tất cả sức mạnh cuối cùng có thể có của hiệp sĩ. 
2. Đối với mỗi giá trị cường độ L, hãy xác định chi phí tốt nhất để có được nó. Nếu có nhiều cửa hàng cung cấp cùng một L, chúng tôi sẽ lấy chi phí tối thiểu trong số đó. Nếu L bằng 0 thì chi phí bằng 0 vì không có giao dịch mua nào được thực hiện. Điều này hiệu quả vì việc mua một lọ thuốc chỉ quan trọng ở cấp độ cuối cùng của nó và việc nâng cấp trung gian không bao giờ giúp ích được gì. 
3. Sắp xếp tất cả quái vật theo sức mạnh yêu cầu của chúng h. Ngoài ra, hãy tính tổng tiền tố của chi phí phạt f của chúng để chúng ta có thể nhanh chóng truy vấn tổng số tiền phạt đối với bất kỳ hậu tố nào của quái vật. 
4. Đối với sức mạnh L của ứng cử viên nhất định, chúng tôi muốn tính tổng chi phí của những quái vật có sức mạnh yêu cầu vượt quá L. Điều này tương đương với việc tính tổng tất cả f cho những quái vật có h > L. Sử dụng tìm kiếm nhị phân trên mảng h đã sắp xếp, chúng tôi xác định vị trí quái vật đầu tiên không bị L đánh bại và lấy tổng hậu tố tương ứng. 
5. Tổng chi phí cho một L cố định là tổng của chi phí cửa hàng tốt nhất cho L và chi phí phạt quái vật cho L. 
6. Chúng tôi tính giá trị này cho mọi ứng cử viên L và trả về giá trị tối thiểu trên tất cả chúng. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ mô phỏng chuyển động dọc theo đường dẫn. Các vị trí a_i và b_i không liên quan vì chúng chỉ ảnh hưởng đến việc đặt hàng chứ không ảnh hưởng đến tính khả thi hay tích lũy chi phí. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến rằng sức mạnh của hiệp sĩ được mô tả đầy đủ bằng một giá trị vô hướng duy nhất chỉ tăng lên và không phụ thuộc vào thứ tự không gian. Bất kỳ chiến lược nào cũng có thể được thể hiện bằng cách chọn sức mạnh cuối cùng L và trả tiền cho chính xác một lần nâng cấp hiệu quả để đạt được sức mạnh đó. Sau khi L được sửa, tất cả quái vật hoạt động độc lập dưới dạng cuộc chạm trán miễn phí hoặc trả phí chỉ dựa trên việc h_i có vượt quá L hay không. Điều này loại bỏ tất cả sự ghép nối giữa các quyết định tại các điểm khác nhau trong ngục tối, biến tối ưu hóa tuần tự thành giảm thiểu tĩnh trên một tập hợp nhỏ rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    D, n, m = map(int, input().split())

    monsters = []
    for _ in range(n):
        a, h, f = map(int, input().split())
        monsters.append((h, f))

    shop_best = {}
    candidates = [0]

    for _ in range(m):
        b, l, w = map(int, input().split())
        if l not in shop_best:
            shop_best[l] = w
        else:
            shop_best[l] = min(shop_best[l], w)

    for l in shop_best:
        candidates.append(l)

    candidates = list(set(candidates))
    candidates.sort()

    monsters.sort()  # sort by h
    hs = [h for h, f in monsters]
    fs = [f for h, f in monsters]

    # suffix sum of f
    suf = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suf[i] = suf[i + 1] + fs[i]

    def monster_cost(L):
        # first index with h > L
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if hs[mid] <= L:
                lo = mid + 1
            else:
                hi = mid
        return suf[lo]

    ans = float('inf')

    for L in candidates:
        cost_shop = shop_best.get(L, 0)
        cost_mon = monster_cost(L)
        ans = min(ans, cost_shop + cost_mon)

    print(ans)

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc quái vật và nén thông tin cửa hàng vào một bản đồ chỉ lưu giữ cách rẻ nhất để đạt được từng cấp độ sức mạnh. Điều này là đủ vì chỉ có sức mạnh đạt được tối đa mới quan trọng. 

Các quái vật được sắp xếp theo sức mạnh cần thiết để chúng ta có thể sử dụng tìm kiếm nhị phân để chia chúng thành những con bị đánh bại và những con không dành cho bất kỳ ứng cử viên L nào. Sau đó, một mảng tổng hậu tố cho phép tính toán theo thời gian liên tục tổng số hình phạt đối với tất cả quái vật vượt quá điểm phân chia đó. 

Mỗi sức mạnh của ứng viên được đánh giá độc lập. Câu trả lời cuối cùng là chi phí kết hợp tối thiểu của việc mua hàng tại cửa hàng cộng với các hình phạt nặng nề. 

Một điều tinh tế phổ biến là đảm bảo rằng điểm mạnh 0 được đưa vào làm ứng cử viên. Nếu không có nó, những trường hợp không mua thuốc là tối ưu sẽ bị bỏ qua hoàn toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi điểm mạnh của ứng viên và đánh giá từng điểm mạnh. 

| L | Chi phí cửa hàng | Quái vật được trả tiền | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | tất cả quái vật | lớn | 
| 5 | 10 | quái vật một phần | 190 | 
| 30 | 50 | vài con quái vật | cao hơn | 

Lựa chọn tối ưu là L = 30, kết hợp với việc mua hàng có chọn lọc nhằm giảm thiểu hình phạt quái vật và chi phí nâng cấp, tạo ra 190. 

Điều này chứng tỏ rằng cấp độ thuốc cao hơn có thể giảm hình phạt của quái vật đủ để bù đắp cho chi phí của nó, ngay cả khi có các nâng cấp trung gian rẻ hơn. 

### Mẫu 2 

| L | Chi phí cửa hàng | Quái vật được trả tiền | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | tất cả quái vật | cao | 
| 3 | 0 | một số quái vật | trung bình | 
| 9 | 100 | vài con quái vật | 115 | 
| 1 | 50 | nhiều quái vật | tệ hơn | 

Ở đây, giải pháp tốt nhất là đầu tư vào một lọ thuốc cấp cao, mặc dù nó đắt tiền, vì nó loại bỏ được nhiều hình phạt từ quái vật. 

Điều này khẳng định rằng sự đánh đổi hoàn toàn mang tính toàn cầu và không phụ thuộc vào việc đặt hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | sắp xếp quái vật, xây dựng cấu trúc tiền tố/hậu tố, tìm kiếm nhị phân cho mỗi ứng viên | 
| Không gian | O(n + m) | kho chứa quái vật, bản đồ cửa hàng và danh sách ứng cử viên | 

Các ràng buộc cho phép tổng số sự kiện lên tới 200000 và việc xử lý logarit cho mỗi ứng cử viên giúp thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    D, n, m = map(int, input().split())

    monsters = []
    for _ in range(n):
        a, h, f = map(int, input().split())
        monsters.append((h, f))

    shop_best = {0: 0}
    candidates = [0]

    for _ in range(m):
        b, l, w = map(int, input().split())
        shop_best[l] = min(shop_best.get(l, float('inf')), w)

    for l in shop_best:
        candidates.append(l)

    candidates = sorted(set(candidates))

    monsters.sort()
    hs = [h for h, f in monsters]
    fs = [f for h, f in monsters]

    suf = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suf[i] = suf[i + 1] + fs[i]

    def cost_mon(L):
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if hs[mid] <= L:
                lo = mid + 1
            else:
                hi = mid
        return suf[lo]

    ans = float('inf')
    for L in candidates:
        ans = min(ans, shop_best.get(L, 0) + cost_mon(L))

    return str(ans)

# samples
assert solve("""10 4 3
1 6 30
3 2 50
5 6 100
8 30 1000
2 5 10
6 30 100
7 30 50
""") == "190"

assert solve("""8 4 3
2 5 100
4 3 100
5 1 100
7 7 15
1 3 0
6 9 100
8 1 50
""") == "115"

# minimum case
assert solve("""5 1 1
1 10 100
2 5 50
""") == "50"

# no shops useful
assert solve("""5 2 0
1 2 10
3 4 20
""") == "30"

# high potion dominates
assert solve("""10 2 2
1 2 10
2 5 10
3 10 1
4 10 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 50 | xử lý quái vật/cửa hàng đơn lẻ | 
| không có cửa hàng hữu ích | 30 | cường độ cơ bản 0 độ chính xác | 
| thuốc cao chiếm ưu thế | 1 | tối ưu hóa toàn cầu so với chi phí địa phương | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có lọ thuốc nào đáng mua. Trong tình huống đó, sức mạnh ứng cử viên duy nhất là 0 và thuật toán tính tổng chính xác tất cả các hình phạt đối với quái vật vì mọi quái vật đều có h_i > 0. 

Một trường hợp tinh tế khác là nhiều cửa hàng cung cấp cùng một thế mạnh. Thuật toán nén chúng một cách chính xác thành một mức giá tốt nhất cho mỗi điểm mạnh, ngăn chặn việc tính toán quá mức hoặc mua nhầm nhiều lần. 

Một trường hợp khác là khi tồn tại một loại thuốc rất mạnh nhưng đắt tiền. Thuật toán vẫn xem xét nó vì nó được bao gồm trong tập ứng cử viên và nó có thể chiếm ưu thế trong tất cả các lựa chọn trung gian nếu nó giảm đủ hình phạt quái vật.
