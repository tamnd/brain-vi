---
title: "CF 102219F - Hạng quân sự"
description: "Chúng tôi có hai hàng (n) lính. Việc so khớp sẽ chọn chính xác một người lính từ hàng thứ hai cho mỗi người lính ở hàng đầu tiên, với mỗi người lính ở hàng thứ hai được sử dụng đúng một lần."
date: "2026-08-18T23:37:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "F"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 746
verified: false
draft: false
---

[CF 102219F - Cấp quân sự](https://codeforces.com/problemset/problem/102219/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai hàng (n) lính. Việc so khớp sẽ chọn chính xác một người lính từ hàng thứ hai cho mỗi người lính ở hàng đầu tiên, với mỗi người lính ở hàng thứ hai được sử dụng đúng một lần. Vì vậy, câu trả lời là số hoán vị hợp lệ (p) của (1,\ldots,n) sao cho người lính (i) ở hàng đầu tiên khớp với người lính (p_i) ở hàng thứ hai, (|i-p_i|\le e), và không có cặp nào bị cấm rõ ràng ((u_i,v_i)) được sử dụng. 

Hạn chế thú vị không chỉ là (e) nhỏ. Nó cho biết mọi người lính (i) chỉ có thể tương tác với các vị trí ở hàng thứ hai trong khoảng thời gian ngắn ([i-e,i+e]). Vì (e\le4), có nhiều nhất (9) vị trí ở hàng thứ hai phù hợp cho bất kỳ người lính ở hàng đầu tiên nào. Số lượng binh sĩ hàng đầu có thể lên tới (2000) nên cần có thuật toán đa thức. Một thuật toán (O(n^2)) đã có khoảng bốn triệu phép tính cơ bản, điều này là hợp lý, trong khi bất kỳ thuật toán nào theo cấp số nhân trong (n) là không thể. Hằng số nhỏ (e) là đặc điểm cho phép chúng ta giữ một không gian trạng thái hàm mũ có kích thước phụ thuộc vào (e), không phụ thuộc vào (n). 

Việc triển khai ngây thơ cũng có thể thất bại ở ranh giới. Ví dụ, với```
1 0 0
```có chính xác một kết quả phù hợp, vì vậy câu trả lời là (1). Một DP mù quáng đảm nhận các vị trí (i-e,\ldots,i+e) đều tồn tại có thể vô tình coi một người lính ở hàng thứ hai không tồn tại là có thể sử dụng được. 

Một trường hợp ranh giới khác là```
2 1 0
```câu trả lời của họ là (2). Cả hai kết quả đều có thể thực hiện được: (1\to1,2\to2) và (1\to2,2\to1). Việc triển khai cửa sổ trượt bất cẩn có thể làm mất một trong các trạng thái này khi cửa sổ di chuyển qua đầu bên phải. 

Các cặp bị cấm chỉ được áp dụng khi lính hàng đầu tương ứng được xử lý. Ví dụ,```
2 1 1
1 2
```có câu trả lời (1), vì việc khớp (1\to2,2\to1) bị cấm trong khi (1\to1,2\to2) vẫn hợp lệ. Nếu cặp bị cấm được lưu trữ trên toàn cầu mà không kết nối nó với đúng hàng của DP thì rất dễ loại bỏ quá trình chuyển đổi sai. 

Trường hợp (e=0) cũng có cấu trúc khác. Với```
3 0 0
```câu trả lời là (1), bởi vì mỗi người lính đều có chính xác một đối tác khả thi. Việc triển khai chuyển đổi trạng thái giả định ít nhất hai vị trí ứng cử viên có thể đưa ra các chuyển đổi không hợp lệ hoặc xử lý sai mặt nạ một bit. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất là xây dựng từng người lính ở hàng đầu tiên phù hợp. Đối với người lính (i), nó thử mọi người lính hàng thứ hai (j) thỏa mãn (|i-j|\le e), kiểm tra xem (j) đã được sử dụng chưa và tiếp tục đệ quy. Khi tất cả (n) binh sĩ đã được xử lý, một kết quả khớp hoàn chỉnh đã được tìm thấy. 

Điều này đúng vì mọi kết quả phù hợp có thể tương ứng với chính xác một chuỗi lựa chọn và đệ quy sẽ từ chối chính xác một lựa chọn khi nó vi phạm giới hạn khoảng cách, cặp bị cấm hoặc yêu cầu mỗi người lính ở hàng thứ hai phải được sử dụng một lần. 

Vấn đề là số lượng các trạng thái được khám phá. Mặc dù mỗi người lính có nhiều nhất (2e+1\le9) ứng viên, nhưng tìm kiếm đệ quy có giới hạn trên theo cấp số nhân là (9^n). Đối với (n=2000), giới hạn đó là (9^{2000}), gần đúng (10^{1908}), vượt xa mọi số lượng thao tác thực tế. Một cách tiếp cận bạo lực thậm chí còn đơn giản hơn để tạo ra tất cả (n!) hoán vị sẽ thực hiện kiểm tra cặp (n\cdot n!), điều này thậm chí còn tệ hơn. 

Lực lượng vũ phu hoạt động vì đối tác hợp pháp của một người lính là người địa phương, nhưng nó liên tục tính toán lại các tình huống từng phần giống nhau. Giả sử chúng ta đã xử lý các lính (1,\ldots,i-1). Đối với những người lính tương lai, chúng ta không cần phải nhớ toàn bộ lịch sử về những người lính ở hàng thứ hai đã được sử dụng. Chúng ta chỉ cần biết vị trí nào trong khoảng cục bộ hiện tại đang được chiếm giữ. 

Quan sát quan trọng là một người lính ở hàng đầu trong tương lai không bao giờ có thể sử dụng vị trí ở hàng thứ hai nhiều hơn (e) vị trí phía sau nó. Khi chúng ta chuyển từ lính (i) sang lính (i+1), vị trí ngoài cùng bên trái trong cửa sổ hiện tại sẽ vĩnh viễn rời khỏi cửa sổ. Nó chắc chắn đã được khớp rồi. Đồng thời, chỉ có một vị trí mới vào cửa sổ. 

Điều này mang lại một mặt nạ bit DP cửa sổ trượt. Cửa sổ có nhiều nhất là (2e+1) vị trí (9), do đó có nhiều nhất 

[ 
2^{2e+1}\le2^9=512 
] 

các trạng thái có thể. Chúng tôi xử lý tất cả (n) binh sĩ ở hàng đầu tiên và đối với mỗi tiểu bang, hãy thử tối đa (2e+1\le9) đối tác ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((2e+1)^n)) trong tìm kiếm được cắt tỉa | (O(n)) độ sâu đệ quy | Quá chậm | 
| Tối ưu | (O(n(2e+1)2^{2e+1})) | (O(2^{2e+1}+k+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định cửa sổ trượt (2e+1) vị trí hàng thứ hai xung quanh người lính hàng đầu hiện tại (i). Bit (b) thể hiện vị trí 

[ 
j=i-e+b. 
] 

Một bit bằng (1) có nghĩa là vị trí này đã không sẵn có, vì nó đã được khớp trước đó hoặc vì nó nằm ngoài phạm vi thực tế (1,\ldots,n). Việc xử lý các vị trí bên ngoài mảng như đã được chiếm giữ sẽ cho phép logic chuyển tiếp giống nhau hoạt động ở cả hai đầu của mảng. 

1. Trước khi xử lý lính (1), hãy tạo mặt nạ ban đầu cho cửa sổ 

[ 
1-e,\ldots,1+e. 
] 

Mọi vị trí bên dưới (1) đều được đánh dấu là đã có người sử dụng. Mọi vị trí thực ban đầu đều miễn phí trừ khi nó đã bị loại trừ bởi vấn đề này, mặc dù các cặp bị cấm được xử lý như các hạn chế chuyển tiếp chứ không phải như các vị trí được chiếm giữ ban đầu. 

1. Đối với mỗi người lính hàng đầu (i), hãy tạo một bitmask`allowed`chứa các vị trí (j) trong cửa sổ hiện tại mà (1\le j\le n), (|i-j|\le e) và ((i,j)) không bị cấm. 
2. Đối với mỗi mặt nạ DP có thể tiếp cận, hãy tìm các vị trí ứng cử viên miễn phí với 

[ 
\text{choices}=\text{allowed}\ &\ \sim\text{mask}. 
] 

Mỗi bộ bit trong`choices`đại diện cho một đối tác có thể có cho người lính (i). Việc chọn bit đó tương ứng với việc tạo một phần mở rộng có thể có của mọi kết quả khớp từng phần được biểu thị bằng trạng thái hiện tại. 

1. Sau khi chọn đối tác, hãy kiểm tra phần ngoài cùng bên trái của mặt nạ thu được. Vị trí ngoài cùng bên trái là (i-e). Sau khi người lính (i) đã được khớp, không người lính hàng đầu nào trong tương lai có thể sử dụng vị trí đó. Do đó, nếu bit đó vẫn bằng 0 thì việc khớp từng phần không bao giờ có thể hoàn thành và phải bị loại bỏ. 

Đây là kiểm tra tính hợp lệ trung tâm của DP cửa sổ trượt. Nó ngăn chúng tôi hoãn một trận đấu ngoài người lính hàng đầu cuối cùng có thể sử dụng hợp pháp một vị trí cụ thể ở hàng thứ hai. 

1. Dịch chuyển mặt nạ sang phải một chút. Vị trí cũ (i-e) biến mất, mọi vị trí còn lại di chuyển một bit về phía bên trái và vị trí mới (i+e+1) đi vào ở bit cao nhất. 

Nếu vị trí mới lớn hơn (n), thì nó nằm ngoài hàng thứ hai thực, do đó bit của nó được chèn vào là (1). Ngược lại, nó được chèn vào dưới dạng (0), vì không có người lính nào trước đó có thể sử dụng vị trí mới được giới thiệu này. 

1. Lưu trữ số lượng khớp một phần cho mặt nạ kết quả trong mảng DP tiếp theo. Tất cả các phép cộng được thực hiện theo modulo (10^9+7). 
2. Sau khi xử lý tất cả (n) binh sĩ hàng đầu, trạng thái cuối cùng hợp lệ duy nhất là mặt nạ tất cả mọi người. Mọi vị trí thực tế ở hàng thứ hai đều phải được sử dụng, trong khi mọi vị trí bên ngoài (1,\ldots,n) cũng được đánh dấu là đã được sử dụng. Giá trị của trạng thái này là câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Sau khi xử lý các lính hàng đầu tiên (1,\ldots,i), bất biến DP là`dp[mask]`đếm chính xác sự trùng khớp một phần của những người lính có vị trí hàng thứ hai đã sử dụng và không có sẵn trong cửa sổ hiện tại được mô tả bởi`mask`. Các vị trí đã rời khỏi cửa sổ không còn cần thiết nữa và quá trình chuyển đổi yêu cầu rõ ràng vị trí đi phải được chiếm giữ trước khi loại bỏ nó. Mỗi đối tác hợp pháp được biểu thị bằng một bit được phép miễn phí, do đó, mọi kết quả khớp một phần hợp lệ đều có chính xác các phần tiếp theo có thể được biểu thị bằng các chuyển đổi, không bị trùng lặp. Cuối cùng, trạng thái tất cả một có nghĩa là mỗi người lính ở hàng thứ hai thực sự đã được ghép đúng một lần, vì vậy số lượng của nó chính xác là số lượng khớp hoàn toàn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, e, k = map(int, input().split())

    # bad[i] is a bitmask of second-row positions forbidden for first-row i.
    bad = [0] * (n + 1)

    for _ in range(k):
        u, v = map(int, input().split())
        # v is relevant only if it can be within e of u.
        d = v - (u - e)
        if 0 <= d <= 2 * e:
            bad[u] |= 1 << d

    width = 2 * e + 1
    states = 1 << width
    top_bit = 1 << (width - 1)
    full = states - 1

    # For every first-row position, build the set of legal real
    # second-row positions in its current window.
    allowed = [0] * (n + 1)

    for i in range(1, n + 1):
        mask = 0
        base = i - e
        for b in range(width):
            j = base + b
            if 1 <= j <= n and not (bad[i] >> b & 1):
                mask |= 1 << b
        allowed[i] = mask

    # Initial window for i = 1 is [1-e, 1+e].
    # Positions <= 0 are outside the array, so mark them occupied.
    start_mask = 0
    base = 1 - e
    for b in range(width):
        j = base + b
        if j < 1 or j > n:
            start_mask |= 1 << b

    dp = [0] * states
    dp[start_mask] = 1

    for i in range(1, n + 1):
        cur_allowed = allowed[i]
        out_of_range = i + e + 1 > n

        ndp = [0] * states

        for mask, ways in enumerate(dp):
            if ways == 0:
                continue

            choices = cur_allowed & ~mask

            while choices:
                bit = choices & -choices
                choices -= bit

                used = mask | bit

                # The position leaving the window must already be used.
                if (used & 1) == 0:
                    continue

                new_mask = used >> 1

                # Introduce position i+e+1.
                if out_of_range:
                    new_mask |= top_bit

                ndp[new_mask] = (ndp[new_mask] + ways) % MOD

        dp = ndp

    print(dp[full] % MOD)

if __name__ == "__main__":
    solve()
```các`bad`mảng lưu trữ các vị trí hàng thứ hai bị cấm dưới dạng bit cục bộ. Đối với cặp bị cấm ((u,v)), vị trí bit là (v-(u-e)), vì bit (0) của cửa sổ hàng (u) đại diện cho (u-e). Điều này giúp việc kiểm tra cạnh cấm diễn ra liên tục. 

các`allowed`mảng được tính toán trước để vòng lặp DP chính không lặp lại việc kiểm tra điều kiện khoảng cách hoặc tìm kiếm cấu trúc cặp cấm. Vì mỗi cửa sổ chứa tối đa chín vị trí nên việc xây dựng tất cả các mặt nạ này chỉ tốn (O(ne)). 

Mặt nạ ban đầu yêu cầu xử lý đặc biệt vì cửa sổ đầu tiên mở rộng đến các vị trí nhỏ hơn (1). Những vị trí như vậy không thể được chọn, do đó các bit của chúng bắt đầu bằng (1). Ý tưởng tương tự được sử dụng khi vị trí mới được nhập từ bên phải: một lần (i+e+1>n), bit mới được chèn vào là (1). 

biểu thức`choices = cur_allowed & ~mask`cô lập mọi vị trí pháp lý chưa được sử dụng. Vòng lặp`bit = choices & -choices`trích xuất một ứng cử viên tại một thời điểm mà không cần quét lại tất cả chín bit. 

Việc kiểm tra bit gửi đi phải diễn ra sau khi thêm đối tác mới được chọn. Nếu bit gửi đi bằng 0, người lính hiện tại không khớp được với vị trí hàng thứ hai sắp trở thành không thể truy cập được. Trạng thái như vậy không thể dẫn đến sự phù hợp hoàn toàn. 

Số nguyên Python không bị tràn, nhưng giá trị DP vẫn giảm modulo (10^9+7) sau mỗi lần cộng. Hai mảng được sử dụng thay vì giữ tất cả (n) lớp, vì chỉ cần lớp trước đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 1 0
```Ở đây cửa sổ có ba vị trí. Đối với người lính đầu tiên, cửa sổ là (0,1,2), do đó vị trí (0) bắt đầu được chiếm giữ. Sau mỗi bước, các vị trí bên ngoài (1,2) cũng được đánh dấu đã chiếm dụng. 

| Lính hàng đầu (i) | Mặt nạ hiện tại | Đối tác được chọn | Mặt nạ mới | 
| --- | --- | --- | --- | 
| 1 |`001`| 1 |`101`| 
| 1 |`001`| 2 |`110`| 
| 2 |`101`| 2 |`111`| 
| 2 |`110`| 1 |`111`| 

Hai đường dẫn tương ứng với (1\to1,2\to2) và (1\to2,2\to1). Cả hai đều kết thúc ở trạng thái tất cả, vì vậy câu trả lời là (2). 

Dấu vết cho thấy tại sao mặt nạ phải bao gồm các vị trí bên ngoài mảng thực tế. Nếu không đánh dấu vị trí (0) và các vị trí ngoài (n) là đã được sử dụng, trạng thái cuối cùng sẽ không thể hiện chính xác rằng tất cả các vị trí thực ở hàng thứ hai đã được sử dụng. 

### Mẫu 2 

Đầu vào là```
2 1 1
1 2
```Cặp (1\to2) bị cấm nên người lính đầu tiên chỉ có một lần chuyển tiếp hợp pháp. 

| Lính hàng đầu (i) | Mặt nạ hiện tại | Lựa chọn được phép | Đối tác được chọn | Mặt nạ mới | 
| --- | --- | --- | --- | --- | 
| 1 |`001`| 1 | 1 |`101`| 
| 2 |`101`| 2 | 2 |`111`| 

Kết quả phù hợp duy nhất còn sót lại là (1\to1,2\to2), vì vậy câu trả lời là (1). 

Dấu vết này chứng tỏ rằng các cặp bị cấm không yêu cầu kích thước DP mới. Họ chỉ cần loại bỏ một bit khỏi tập hợp các chuyển đổi có sẵn cho người lính ở hàng đầu tiên tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n(2e+1)2^{2e+1}+k)) | Có (n) lớp, nhiều nhất (2^{2e+1}) mặt nạ trên mỗi lớp và nhiều nhất (2e+1) đối tác ứng cử viên trên mỗi mặt nạ. | 
| Không gian | (O(2^{2e+1}+n+k)) | Hai mảng DP giữ các trạng thái, trong khi mặt nạ bị cấm và được phép yêu cầu lưu trữ tuyến tính. | 

Với (e\le4), DP có nhiều nhất (2^9=512) trạng thái và nhiều nhất là chín lần chuyển đổi cho mỗi trạng thái. Đối với (n=2000), giới hạn chính là khoảng (2000\cdot512\cdot9), khoảng (9,2) triệu lần chuyển đổi trạng thái, phù hợp với các ràng buộc dự kiến. Việc sử dụng bộ nhớ nhỏ vì chỉ giữ lại hai lớp không gian trạng thái có kích thước không đổi. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng cùng một`solve`thực hiện đúng như chương trình đã đệ trình. Trường hợp kích thước tối đa có chủ ý sử dụng (e=0), trong đó câu trả lời được xác định ngay lập tức bằng kết quả khớp duy nhất có thể, do đó, nó cũng kiểm tra xem việc triển khai có xử lý được không (n=2000).```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n, e, k = map(int, input().split())

    bad = [0] * (n + 1)

    for _ in range(k):
        u, v = map(int, input().split())
        d = v - (u - e)
        if 0 <= d <= 2 * e:
            bad[u] |= 1 << d

    width = 2 * e + 1
    states = 1 << width
    top_bit = 1 << (width - 1)
    full = states - 1

    allowed = [0] * (n + 1)

    for i in range(1, n + 1):
        mask = 0
        base = i - e
        for b in range(width):
            j
```
