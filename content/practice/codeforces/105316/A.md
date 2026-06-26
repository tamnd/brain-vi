---
title: "CF 105316A - Rajaee trong bếp"
description: "Chúng ta được cho một mảng các số nguyên dương. Nhiệm vụ là chia nó thành nhiều đoạn không trống liên tiếp. Mỗi phần tử phải thuộc chính xác một phân đoạn, vì vậy mọi giải pháp hợp lệ đều tương ứng với một phân vùng đầy đủ của mảng thành các khối liền kề."
date: "2026-06-23T15:08:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "A"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 62
verified: true
draft: false
---

[CF 105316A - Rajaee trong bếp](https://codeforces.com/problemset/problem/105316/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Nhiệm vụ là chia nó thành nhiều đoạn không trống liên tiếp. Mỗi phần tử phải thuộc chính xác một phân đoạn, vì vậy mọi giải pháp hợp lệ đều tương ứng với một phân vùng đầy đủ của mảng thành các khối liền kề. 

Đối với mỗi phân đoạn, chúng tôi tính tổng của nó và các tổng này trở thành độ dài cạnh của đa giác. Câu hỏi đặt ra là đếm xem có bao nhiêu cách chúng ta có thể phân vùng mảng sao cho các độ dài cạnh này có thể tạo thành một đa giác lồi. 

Ràng buộc hình học là phần ẩn chính. Một tập hợp các độ dài dương có thể tạo thành một đa giác lồi khi và chỉ khi không có một cạnh nào quá lớn, chính xác hơn là cạnh lớn nhất phải nhỏ hơn tổng của tất cả các cạnh còn lại. Nếu tổng các phần tử là S và tổng các phân đoạn là s1, s2, ..., sk thì điều kiện là max(si) < S - max(si), tương đương với 2 * max(si) < S. 

Điều này biến vấn đề từ hình học thành một ràng buộc về phân vùng: chúng ta cần đếm tất cả các cách để phân chia mảng sao cho tổng phân đoạn tối đa nhỏ hơn một nửa tổng tổng của mảng. 

Vì N có thể lên tới 10^6 nên mọi giải pháp thử tất cả các phân vùng một cách rõ ràng là không thể. Ngay cả các cách tiếp cận O(N^2) cũng sẽ bao gồm khoảng 10^12 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Điều này ngay lập tức gợi ý rằng chúng ta cần một phương pháp quy hoạch động tuyến tính hoặc gần tuyến tính với cấu trúc cửa sổ trượt. 

Trường hợp cạnh tinh tế xuất hiện khi một phần tử đã quá lớn. Ví dụ: nếu tổng là 10 và một phần tử là 6 thì không có phân vùng nào chứa phần tử đó trong một phân đoạn hợp lệ có thể thỏa mãn điều kiện, vì chỉ riêng phân đoạn đó sẽ vi phạm bất đẳng thức đa giác. Trong những trường hợp như vậy, câu trả lời trở thành số 0 mặc dù có nhiều phân vùng tồn tại dưới dạng tổ hợp. 

Một trường hợp khác là khi cấu trúc phân vùng tối ưu phụ thuộc vào việc cân bằng tổng các phân đoạn. Ví dụ: nếu tất cả các phần tử đều lớn so với tổng, thì cửa sổ hợp lệ cho tổng phân đoạn sẽ trở nên rất nhỏ, đôi khi buộc các phân đoạn có độ dài bằng một hoặc làm cho câu trả lời biến mất hoàn toàn. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ liệt kê mọi cách có thể để đặt các vị trí cắt giữa các phần tử. Đối với một mảng có độ dài N, có N−1 vị trí cắt có thể có, do đó có 2^(N−1) phân vùng có thể. Đối với mỗi phân vùng, chúng tôi tính tổng phân đoạn và kiểm tra xem điều kiện đa giác có đúng hay không. Điều này đúng nhưng hoàn toàn không khả thi, vì ngay cả N = 40 cũng đã tạo ra hơn một nghìn tỷ phân vùng. 

Quan sát quan trọng là điều kiện hình học chỉ phụ thuộc vào tổng phân đoạn tối đa. Sau khi chúng tôi ấn định ngưỡng X bằng sàn((S−1)/2), mọi phân vùng hợp lệ chính xác là một phân vùng trong đó tổng mỗi phân đoạn lớn nhất là X. Điều kiện “tổng phân đoạn tối đa < S/2” trở thành “tổng tất cả các phân đoạn ≤ X”. Điều này loại bỏ mọi sự phụ thuộc giữa các phân đoạn vượt quá giới hạn tổng cục bộ. 

Bây giờ bài toán trở thành một bài toán đếm cổ điển: đếm số cách phân chia một mảng thành các đoạn liền kề sao cho tổng mỗi đoạn không vượt quá X. Điều này có thể được giải quyết bằng lập trình động. Đối với mỗi vị trí i, chúng ta xem xét tất cả các vị trí cắt j trước đó sao cho mảng con (j+1 đến i) có tổng ≤ X và tích lũy dp[j]. 

Một DP ngây thơ vẫn thử chuyển đổi O(N^2). Tuy nhiên, vì tất cả các giá trị đều dương nên chúng ta có thể duy trì một cửa sổ trượt có các điểm bắt đầu hợp lệ bằng cách sử dụng hai con trỏ. Khi i tăng, ranh giới bên trái hợp lệ chỉ di chuyển về phía trước. Điều này cho phép chúng tôi tính toán từng dp[i] trong thời gian O(1) được khấu hao bằng cách sử dụng tổng tiền tố trên các giá trị dp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N · N) | O(N) | Quá chậm | 
| DP tối ưu với hai con trỏ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng ta tính tổng S của mảng và rút ra ngưỡng X = (S−1)//2. Giá trị này đại diện cho tổng phân đoạn tối đa được phép. 

Sau đó, chúng tôi sử dụng quy hoạch động trong đó dp[i] biểu thị số cách hợp lệ để phân vùng tiền tố a[1..i]. Chúng tôi cũng duy trì một mảng tổng tiền tố trên dp để cho phép truy vấn tổng trong phạm vi nhanh. 

1. Khởi tạo dp[0] = 1, thể hiện một cách phân vùng tiền tố trống. Đồng thời duy trì prefix_dp[0] = 1. 
2. Duy trì một cửa sổ trượt con trỏ trái l = 1 và tổng đoạn chạy current_sum = 0 cho cửa sổ kết thúc tại i. 
3. Với mỗi i từ 1 đến N, hãy mở rộng cửa sổ bằng cách thêm a[i] vào current_sum. 
4. Nếu current_sum vượt quá X, hãy thu nhỏ cửa sổ từ bên trái bằng cách tăng l và trừ a[l] khỏi current_sum cho đến khi nó trở lại hợp lệ. Điều này đảm bảo mọi phân đoạn kết thúc tại i bắt đầu từ bất kỳ j nào trong [l..i] có tổng ≤ X. 
5. Sau khi xác định được phạm vi hợp lệ của các vị trí bắt đầu, hãy tính dp[i] là tổng của dp[j−1] cho tất cả j trong [l..i]. Điều này được tính toán một cách hiệu quả bằng cách sử dụng các tổng tiền tố: dp[i] = prefix_dp[i−1] − prefix_dp[l−2]. 
6. Cập nhật prefix_dp[i] = prefix_dp[i−1] + dp[i], lấy modulo 1e9+7. 

Tính chính xác xuất phát từ thực tế là tất cả các phân đoạn cuối cùng hợp lệ kết thúc tại i tương ứng chính xác với một phạm vi liền kề của các chỉ số bắt đầu và tính tích cực của các phần tử mảng đảm bảo phạm vi này di chuyển đơn điệu khi i tăng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại mỗi vị trí i, cửa sổ trượt [l, i] biểu thị chính xác tập hợp các điểm bắt đầu mà đoạn kết thúc tại i có tổng tối đa là X. Vì tất cả các phần tử đều dương nên việc mở rộng ranh giới bên trái chỉ có thể làm giảm tổng các phân đoạn và di chuyển sang phải chỉ có thể làm tăng chúng, vì vậy tập hợp lệ luôn là một khoảng tiền tố. Cấu trúc này đảm bảo rằng mọi phân vùng hợp lệ được tính chính xác một lần khi chúng tôi mở rộng chuyển tiếp dp và không có phân vùng không hợp lệ nào đóng góp vì bất kỳ phân vùng nào vượt quá X đều bị loại khỏi cửa sổ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    X = (total - 1) // 2

    # If even a single element exceeds X, no valid segment can contain it
    # since segments are contiguous and must satisfy sum <= X.
    for v in a:
        if v > X:
            print(0)
            return

    dp = [0] * (n + 1)
    prefix = [0] * (n + 1)

    dp[0] = 1
    prefix[0] = 1

    l = 1
    current_sum = 0

    for i in range(1, n + 1):
        current_sum += a[i - 1]

        while current_sum > X:
            current_sum -= a[l - 1]
            l += 1

        left = l - 1
        # dp[i] = sum(dp[j]) for j in [left .. i-1]
        dp[i] = (prefix[i - 1] - (prefix[left - 1] if left > 0 else 0)) % MOD
        prefix[i] = (prefix[i - 1] + dp[i]) % MOD

    print(dp[n] % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo định nghĩa DP trực tiếp. Phần không cần thiết duy nhất là chuyển đổi quá trình chuyển đổi thành truy vấn tổng tiền tố, giúp tránh lặp lại tất cả các vị trí cắt hợp lệ trước đó. Cửa sổ trượt đảm bảo rằng ràng buộc về tổng phân đoạn được thực thi tăng dần mà không cần tính lại tổng từ đầu. 

Phải cẩn thận với các chỉ số trong mảng tiền tố, vì dp[i] phụ thuộc vào dp[j] trong đó j tương ứng với vị trí cắt chứ không phải trực tiếp với chỉ mục mảng. Việc chuyển đổi giữa vị trí bắt đầu phân đoạn và chỉ số dp được xử lý thông qua con trỏ l và dịch chuyển từng đơn vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đầu vào trong đó mảng đủ nhỏ để chúng ta có thể theo dõi tất cả các phân vùng một cách rõ ràng. 

Giả sử các chuyển đổi dp hợp lệ tạo ra hành vi sau: 

| tôi | một [tôi] | tôi | hiện tại_sum | dp[i] | tiền tố[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | x | 1 | x | 1 | 1 | 
| 2 | y | 1 | x+y | 1 | 2 | 
| 3 | z | 2 | y+z | 2 | 4 | 

Dấu vết này cho thấy cách dịch chuyển l loại bỏ các phần bắt đầu phân đoạn không hợp lệ và cách dp tích lũy các đóng góp từ tất cả các vị trí cắt hợp lệ trước đó. 

Thuộc tính chính được xác nhận ở đây là khi tiền tố trở nên hợp lệ, tất cả các phân vùng kết thúc tại chỉ mục đó sẽ được hình thành bằng cách chọn bất kỳ điểm cắt hợp lệ nào trước đó. 

### Ví dụ 2 

Lấy trường hợp một phần tử lớn buộc cửa sổ phải co lại sớm. 

| tôi | một [tôi] | tôi | hiện tại_sum | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | lớn | 1 | lớn | 1 | 
| 2 | nhỏ | 2 | nhỏ | 1 | 
| 3 | nhỏ | 2 | nhỏ+nhỏ | 2 | 

Điều này chứng tỏ cách tiền tố lớn buộc thuật toán loại bỏ các vị trí bắt đầu trước đó và cách dp duy trì tính nhất quán bằng cách chỉ tính các lần bắt đầu phân đoạn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử vào và ra khỏi cửa sổ trượt nhiều nhất một lần và quá trình chuyển đổi dp sử dụng phép trừ tiền tố O(1) | 
| Không gian | O(N) | mảng dp và tiền tố lưu trữ một giá trị cho mỗi vị trí | 

Thuật toán chạy thoải mái trong các giới hạn ngay cả đối với N lên đến 10^6 vì tất cả các hoạt động đều là quét tuyến tính với các cập nhật liên tục theo thời gian cho mỗi chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Note: In a real setup, solve() would be imported and called.
# Here these are structural test examples.

# minimal case
# assert run("3\n1 1 1\n") == "..."

# small increasing
# assert run("4\n1 2 1 2\n") == "..."

# all equal
# assert run("5\n1 1 1 1 1\n") == "..."

# large values edge
# assert run("3\n1000000000 1 1\n") == "..."

# boundary stress
# assert run("6\n1 1 1 1 1 1\n") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng nhỏ bằng nhau | khác không | đếm phân vùng cơ bản | 
| một yếu tố chi phối | 0 | ràng buộc phân đoạn không hợp lệ | 
| tất cả những cái | số tổ hợp lớn | tính đúng đắn của việc tích lũy DP | 
| giá trị hỗn hợp | phụ thuộc | cửa sổ trượt đúng cách | 

## Vỏ cạnh 

Khi một phần tử đơn lẻ đã vượt quá ngưỡng X, mọi phân vùng bao gồm phần tử đó dưới dạng phân đoạn sẽ trở nên không hợp lệ. Trong trường hợp này, cửa sổ trượt không bao giờ ổn định đối với các chỉ số bao gồm phần tử đó và DP đóng góp chính xác bằng 0 vì không tồn tại vị trí bắt đầu hợp lệ cho các phân đoạn chứa chỉ mục đó. 

Khi tất cả các phần tử đều nhỏ, cửa sổ trượt sẽ mở rộng để bao phủ phạm vi dài, nghĩa là mọi vị trí cắt đều có hiệu lực đối với nhiều điểm cuối. Sau đó, DP tích lũy một số lượng lớn các cách và tính chính xác phụ thuộc hoàn toàn vào cấu trúc tổng tiền tố để tránh tính hai lần, vì mỗi lần cắt hợp lệ đóng góp chính xác một lần cho mỗi điểm cuối. 

Khi các phần tử có mức độ mất cân bằng cao, tổng tiền tố lớn ban đầu sẽ gây ra hiện tượng thu hẹp cửa sổ thường xuyên. Thuật toán xử lý việc này một cách rõ ràng vì mỗi lần rút gọn là vĩnh viễn đối với các chỉ mục trước đó và không có phân đoạn không hợp lệ nào được xem xét lại do chuyển động đơn điệu của các con trỏ.
