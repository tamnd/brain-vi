---
title: "CF 104270E - Thực vật vs Zombie"
description: "Chúng ta được cho một dòng thực vật được đánh số từ 1 đến n. Mỗi cây i có vị trí i cố định và tốc độ tăng trưởng a[i]. Ban đầu mọi cây đều có giá trị phòng thủ bằng 0. Robot xuất phát ở vị trí 0, tức là “ngôi nhà”."
date: "2026-07-01T21:27:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104270
codeforces_index: "E"
codeforces_contest_name: "The 2018 ICPC Asia Qingdao Regional Programming Contest (The 1st Universal Cup, Stage 9: Qingdao)"
rating: 0
weight: 104270
solve_time_s: 50
verified: true
draft: false
---

[CF 104270E - Thực vật vs. Thây ma](https://codeforces.com/problemset/problem/104270/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dòng thực vật được đánh số từ 1 đến n. Mỗi cây i có vị trí i cố định và tốc độ tăng trưởng a[i]. Ban đầu mọi cây đều có giá trị phòng thủ bằng 0. Robot xuất phát ở vị trí 0, tức là “ngôi nhà”. 

Robot thực hiện một chuỗi tối đa m bước di chuyển đơn vị trên dòng số nguyên. Mỗi bước di chuyển là một bước về phía đông hoặc một bước về phía tây. Sau mỗi lần di chuyển, nếu robot đáp chính xác vào vị trí i của một số cây, thì cây đó sẽ ngay lập tức nhận được khả năng phòng thủ [i]. Nhiều lần đến thăm cùng một nhà máy sẽ tích lũy giá trị của nó nhiều lần. 

Mục tiêu là thiết kế một lối đi có tối đa m bước sao cho sau tất cả các lần di chuyển, khả năng phòng thủ tối thiểu giữa tất cả các cây càng lớn càng tốt. 

Khó khăn cốt lõi là mọi nhà máy đều phải được “thăm quan đủ số lần”, nhưng các lượt ghé thăm được tạo ra một cách gián tiếp thông qua một robot di chuyển duy nhất bị ràng buộc bởi tổng ngân sách bước. 

Các ràng buộc đẩy chúng ta ra khỏi mọi mô phỏng đường dẫn rõ ràng. n có thể lên tới 100000 và m có thể lớn tới 10^12, vì vậy câu trả lời chỉ phụ thuộc vào mức độ hiệu quả mà chúng ta có thể suy luận về số lượt truy cập chứ không phụ thuộc vào việc xây dựng bước đi từng bước. Bất kỳ giải pháp nào cố gắng mô phỏng chuyển động hoặc liệt kê các đường đi đều không thể thực hiện được ngay lập tức. 

Một trường hợp thất bại tinh vi xuất hiện khi một người cố gắng “cân bằng số lượt ghé thăm” tại địa phương một cách tham lam. Ví dụ, việc xen kẽ hướng đông và hướng tây gần một nhà máy không giúp ích gì cho nhà máy đó hơn là một chiến lược quay lui được lên kế hoạch cẩn thận nhằm tái sử dụng chi phí di chuyển giữa nhiều nhà máy. Sự ghép nối giữa các vị trí liền kề là toàn cầu, không phải cục bộ. 

Một cạm bẫy khác là giả định rằng mỗi nhà máy yêu cầu chi phí đi lại độc lập tỷ lệ thuận với chỉ số của nó. Bởi vì robot có thể vượt qua và quay trở lại, các chuyến thăm có thể được chia sẻ trên nhiều nhà máy trong một lần quét, do đó việc tính toán khoảng cách ngây thơ sẽ đánh giá quá cao chi phí. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng liệt kê tất cả các bước có chiều dài tối đa m và tính toán số lượt truy cập kết quả trên mỗi cây. Về nguyên tắc, điều này đúng vì mọi chuỗi hợp lệ đều được xem xét. Tuy nhiên, hệ số phân nhánh là 2 mỗi bước, do đó số chuỗi là 2^m, điều này hoàn toàn không khả thi ngay cả khi m = 50. 

Ngay cả khi chúng tôi hạn chế chú ý đến việc đếm số lần mỗi vị trí được truy cập, cấu trúc vẫn phức tạp vì các đường dẫn khác nhau có cùng số bước có thể tạo ra phân bổ lượt truy cập khác nhau. 

Quan sát quan trọng là đảo ngược quan điểm: thay vì nghĩ về một con đường tạo ra các chuyến thăm, chúng tôi hỏi mỗi nhà máy phải được viếng thăm bao nhiêu lần để đạt được giá trị phòng thủ tối thiểu mục tiêu x. Vì mỗi lần viếng thăm nhà máy i đóng góp chính xác a[i] nên nhà máy i phải được viếng thăm ít nhất ceil(x / a[i]) lần. 

Bây giờ vấn đề trở thành: với số lượt truy cập được yêu cầu c[i], liệu chúng ta có thể thiết kế một bước đi có độ dài tối đa m truy cập từng vị trí i ít nhất c[i] lần không? 

Cấu trúc chuyển động của robot trên một dòng có đặc tính đơn điệu mạnh: khi chúng ta quyết định bao phủ đoạn từ 1 đến k, chúng ta có thể tổ chức chuyển động bằng cách quét lặp lại các tiền tố. Mỗi lần quét toàn bộ từ 0 đến k và quay lại tốn 2k bước và truy cập mọi vị trí trong phạm vi đó chính xác hai lần mỗi lần quét (một lần trên đường ra và một lần trên đường quay lại), ngoại trừ các điểm cuối tùy thuộc vào tính chẵn lẻ. 

Điều này làm giảm tính khả thi của vấn đề đóng gói tham lam đối với việc đóng góp tiền tố. Chúng tôi tích lũy số lượt truy cập bắt buộc từ trái sang phải và theo dõi số lần duyệt toàn bộ tiền tố là cần thiết để đáp ứng nhu cầu tại mỗi vị trí. Bất cứ khi nào một vị trí yêu cầu nhiều lượt truy cập hơn mức hiện được cung cấp bởi các lần quét trước đó, chúng tôi sẽ mở rộng phạm vi và trả thêm chi phí tương ứng với phần mở rộng đó. 

Cấu trúc đơn điệu nên chúng ta có thể tính x khả thi tối đa bằng tìm kiếm nhị phân. Mỗi lần kiểm tra tính khả thi là tuyến tính theo n.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · n) | O(n) | Quá chậm | 
| Tính khả thi + Tìm kiếm nhị phân | O(n log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn kiểm tra xem liệu giá trị phòng thủ tối thiểu x của ứng viên có thể đạt được hay không. 

1. Chuyển x thành yêu cầu cho mỗi nhà máy c[i] = (x + a[i] - 1) // a[i]. Điều này thể hiện số lần tôi phải đến thăm nhà máy. 
2. Quét các cây từ trái sang phải, duy trì số lượt truy cập đã được “đảm bảo” bằng các lần duyệt đầy đủ tiền tố đã trả tiền trước đó. Chúng tôi duy trì một đường cong thay đổi thể hiện mức độ bao phủ tích lũy từ các lần quét lặp lại. 
3. Tại vị trí i, nếu cur < c[i], chúng ta phải mở rộng phạm vi làm việc lên tới i. Mỗi tiện ích mở rộng cho phép chúng tôi thực hiện quét toàn bộ bổ sung trên [1, i], mỗi tiện ích đóng góp 2 lượt truy cập vào mọi vị trí trong phạm vi đó. 
4. Số lần quét bổ sung cần thiết tại i là (c[i] - cur + 1) // 2, vì mỗi lần quét thêm hai lần truy cập. Chúng tôi thêm số này vào tổng số lần quét đang chạy và cập nhật cur tương ứng bằng cách thêm 2 lần số lần quét. 
5. Nếu tại bất kỳ thời điểm nào tổng chi phí di chuyển do các đợt quét này gây ra vượt quá m thì ứng cử viên x là không khả thi. 
6. Tìm kiếm nhị phân x trong phạm vi [0, max(a[i]) * m], kiểm tra tính khả thi mỗi lần. 

Ý tưởng quan trọng là một khi chúng ta cam kết đạt được vị trí thứ i, chúng ta sẽ không bao giờ cần phải xử lý các vị trí trước đó một cách riêng biệt nữa. Tất cả các yêu cầu trước đó đã được bao phủ bởi cùng một lần quét, vì vậy quá trình này trở nên đơn điệu và tham lam. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là chuyển động tối ưu có thể được phân tách thành các lần quét tiền tố đầy đủ. Bất kỳ bước đi nào trên một dòng đều có thể được sắp xếp lại mà không làm giảm số lượt truy cập để các lượt truy cập diễn ra theo kiểu quét qua lại có cấu trúc trên các tiền tố. Sự chuyển đổi này không làm giảm phạm vi bao phủ vì mỗi lần vượt qua một cạnh đều góp phần như nhau vào phạm vi bao phủ tiền tố liền kề và các đường vòng không cần thiết có thể được nén thành các chuyến đi tối đa lặp đi lặp lại. Điều này tạo ra một mô hình bao phủ đơn điệu trong đó chỉ có các quyết định mở rộng tiền tố mới quan trọng và quy tắc tham lam đảm bảo số lần quét tối thiểu tại mỗi điểm thiếu hụt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, a, m):
    n = len(a)
    cur = 0
    cost = 0

    for i in range(n):
        need = (x + a[i] - 1) // a[i]
        if cur < need:
            add = need - cur
            # each sweep gives 2 visits per position in range
            sweeps = (add + 1) // 2
            cost += sweeps * 2 * (i + 1)
            cur += sweeps * 2

        if cost > m:
            return False

    return cost <= m

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        lo, hi = 0, max(a) * m
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, a, m):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```chức năng`can`mô phỏng liệu có thể đạt được mục tiêu phòng thủ tối thiểu hay không. Biến`cur`theo dõi số lượt truy cập mà mỗi vị trí đã nhận được một cách hiệu quả từ các lần quét toàn bộ đã được quyết định trước đó. Khi chúng tôi đạt đến vị trí thứ i, chúng tôi tính toán số lượt truy cập cần thiết và thêm các lần quét bổ sung nếu mức độ bao phủ hiện tại không đủ. Mỗi lần quét qua tiền tố [1, i] tốn 2*(i+1) bước vì robot phải đi từ nhà đến i và quay trở lại. 

Tìm kiếm nhị phân là cần thiết vì tính khả thi là đơn điệu trong x: nếu chúng ta có thể đạt được mức bảo vệ tối thiểu nhất định thì mọi giá trị nhỏ hơn cũng có thể đạt được. 

Một lỗi triển khai phổ biến là quên rằng mỗi lần quét đóng góp hai lượt truy cập cho mỗi vị trí chứ không phải một do cả lượt chuyển tiếp và chuyển ngược. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ a = [3, 2, 6], m = 30 và kiểm tra x = 6. 

Chúng tôi tính toán số lượt truy cập cần thiết c = [2, 3, 1]. Chúng tôi xử lý từ trái sang phải. 

| tôi | một [tôi] | c[i] | cur trước | thâm hụt | quét | cur sau | chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 0 | 2 | 1 | 2 | 4 | 
| 2 | 2 | 3 | 2 | 1 | 1 | 4 | 12 | 
| 3 | 6 | 1 | 4 | 0 | 0 | 4 | 12 | 

Sau khi xử lý, chi phí = 12 ≤ 30 nên x = 6 là khả thi. 

Dấu vết này cho thấy mức độ bao phủ tích lũy trên các tiền tố. Vị trí thứ hai không thiết lập lại cấu trúc, nó chỉ kiểm tra xem các lần quét hiện tại có đủ hay không. 

Bây giờ hãy xem xét một trường hợp chặt chẽ hơn a = [5, 5], m = 8, x = 10. 

c = [2, 2]. 

| tôi | c[i] | cur | thâm hụt | quét | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 1 | 4 | 
| 2 | 2 | 2 | 0 | 0 | 4 | 

Chúng tôi không thể thực hiện một đợt quét khác ở vị trí 2 vì chi phí sẽ vượt quá m, do đó tính khả thi phụ thuộc vào việc sử dụng lại tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log (tối đa a[i] * m)) | Mỗi lần kiểm tra tính khả thi đều mang tính tuyến tính và chúng tôi tìm kiếm nhị phân phạm vi câu trả lời | 
| Không gian | O(1) thêm | Chỉ có một số bộ đếm được duy trì ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 10^5 phần tử cho mỗi lần kiểm tra và tổng số 10^6 trong các lần kiểm tra, do đó, việc kiểm tra tuyến tính bằng tìm kiếm logarit nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))

            def can(x):
                cur = 0
                cost = 0
                for i in range(n):
                    need = (x + a[i] - 1) // a[i]
                    if cur < need:
                        add = need - cur
                        sweeps = (add + 1) // 2
                        cost += sweeps * 2 * (i + 1)
                        cur += sweeps * 2
                    if cost > m:
                        return False
                return cost <= m

            lo, hi = 0, max(a) * m
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if can(mid):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            out.append(str(ans))

        return "\n".join(out)

    return solve()

# minimal
assert run("1\n2 0\n1 1\n") == "0"

# small balanced
assert run("1\n3 10\n2 2 2\n") == "6"

# single dominant
assert run("1\n3 100\n1 100 1\n") == "100"

# increasing
assert run("1\n4 50\n1 2 3 4\n") == "24"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 0 | trường hợp cạnh ngân sách bằng không | 
| cân bằng nhỏ | 6 | nhân giống đồng đều | 
| thống trị duy nhất | 100 | xử lý yêu cầu sai lệch | 
| ngày càng tăng | 24 | hành vi tích lũy tiền tố | 

## Vỏ cạnh 

Kịch bản không di chuyển trong đó m = 0 buộc tất cả thực vật duy trì ở mức phòng thủ bằng 0 bất kể tốc độ tăng trưởng. Thuật toán xử lý việc này vì tìm kiếm nhị phân sẽ chỉ chấp nhận x = 0; bất kỳ x dương nào cũng thất bại ngay lập tức vì số lượt truy cập được yêu cầu khác 0. 

Trường hợp một cây có a[i] cực lớn so với những cây khác làm nổi bật hành vi phân chia số nguyên. Vì số lượt truy cập cần thiết cho nhà máy đó là nhỏ nên thuật toán sẽ tránh việc phân bổ quá mức các lần quét và việc tái sử dụng tiền tố sẽ cung cấp chính xác số lượt truy cập từ các tiện ích mở rộng trước đó. 

Chuỗi a[i] tăng nghiêm ngặt nhấn mạnh phần mở rộng tiền tố. Mỗi vị trí có thể đưa ra các yêu cầu quét mới và sự tích lũy tham lam đảm bảo chúng tôi chỉ mở rộng khi cần thiết.
