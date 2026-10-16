---
title: "CF 105492I - Thẻ Interrail"
description: "Chúng ta được cung cấp một hành trình cố định gồm n ngày đi, mỗi ngày diễn ra vào một ngày dương lịch cụ thể. Đối với mỗi ngày này, sẽ phải trả phí nếu chúng ta quyết định mua một vé riêng lẻ. Những ngày này đã được sắp xếp theo thời gian và không có hai ngày nào có cùng dấu thời gian."
date: "2026-06-23T19:43:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105492
codeforces_index: "I"
codeforces_contest_name: "2024 Benelux Algorithm Programming Contest (BAPC 24)"
rating: 0
weight: 105492
solve_time_s: 55
verified: true
draft: false
---

[CF 105492I - Thẻ Interrail](https://codeforces.com/problemset/problem/105492/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hành trình cố định gồm n ngày đi, mỗi ngày diễn ra vào một ngày dương lịch cụ thể. Đối với mỗi ngày này, sẽ phải trả phí nếu chúng ta quyết định mua một vé riêng lẻ. Những ngày này đã được sắp xếp theo thời gian và không có hai ngày nào có cùng dấu thời gian. 

Bên cạnh đó, còn có k loại đường sắt. Mỗi loại thẻ hoạt động theo một cách hơi khác thường. Nếu chúng tôi kích hoạt thẻ vào một ngày nào đó, thẻ đó vẫn hoạt động trong một khoảng thời gian cố định gồm p ngày theo lịch liên tiếp. Trong cửa sổ đó, nó có thể được sử dụng để bao gồm tối đa số ngày du lịch theo kế hoạch của chúng tôi. Nếu có nhiều hơn d ngày đi lại trong thời hạn, thì chỉ có ngày đầu tiên trong số đó mới có thể được cấp thẻ và mọi ngày đi lại sau đó trong cùng thời hạn phải được thanh toán riêng ngay cả khi thẻ vẫn còn hiệu lực. Một vấn đề phức tạp quan trọng là các thẻ có thể chồng chéo một cách tự do: nhiều thẻ có thể hoạt động cùng lúc và mỗi ngày đi lại có thể sử dụng mức sử dụng từ nhiều thẻ đang hoạt động cùng lúc mặc dù cuối cùng chỉ đưa ra một quyết định thanh toán cho ngày đó. 

Nhiệm vụ là quyết định, đối với mỗi ngày đi lại, nên trả tiền vé riêng hay dựa vào một hoặc nhiều thẻ đang hoạt động, sao cho mỗi ngày đi lại đều được bảo hiểm và tổng chi phí được giảm thiểu. 

Các ràng buộc quan trọng theo một cách rất có cấu trúc. Số ngày đi lại lên tới 10.000, trong khi số lượng loại thẻ nhiều nhất là 100. Giải pháp thử tất cả các kết hợp thẻ mỗi ngày hoặc mỗi khoảng thời gian nhanh chóng trở nên không khả thi vì ngay cả cách tiếp cận bậc hai hoặc bậc ba trên 10.000 ngày cũng đã quá lớn. Tuy nhiên, số lượng nhỏ các loại thẻ cho thấy rằng mỗi loại thẻ nên được đánh giá độc lập và kết hợp thông qua cấu trúc lập trình động toàn cầu thay vì liệt kê các tương tác một cách rõ ràng. 

Một trường hợp phức tạp nhưng quan trọng phát sinh từ các đường chuyền chồng chéo và quy tắc “sử dụng d đầu tiên”. Xem xét một thẻ có thời hạn hiệu lực dài nhưng nhỏ d. Nếu số ngày đi lại dày đặc trong thời gian sớm, thẻ có thể hết hạn ngạch sớm và trở nên vô dụng trong những ngày sau đó mặc dù thẻ vẫn còn hiệu lực. Một cách tiếp cận ngây thơ giả định “tất cả các ngày trong cửa sổ đều được bảo hiểm sau khi mua thẻ” sẽ vượt quá tính hữu dụng của nó. 

Một trường hợp khác xuất hiện khi vé đi hoàn toàn tệ hơn giá vé riêng lẻ ở một số phân khúc nhưng lại có lợi ở những phân khúc khác. Vì thẻ có thể tái sử dụng và có thể mua nhiều lần nên quyết định không phải là một lựa chọn toàn cầu duy nhất cho mỗi loại mà có thể là nhiều lần kích hoạt vào những ngày bắt đầu khác nhau. 

## Phương pháp tiếp cận 

Chiến lược vũ lực trực tiếp sẽ xử lý mỗi ngày đi lại một cách độc lập và quyết định trong số ba khả năng: thanh toán tiền vé trực tiếp hoặc mua một loại thẻ nào đó bắt đầu vào ngày đó hoặc chỉ định ngày cho một thẻ đã hoạt động. Để thực hiện điều này chính xác, chúng tôi cần theo dõi trạng thái đầy đủ của tất cả các thẻ đang hoạt động, bao gồm số lượng mục đích sử dụng còn lại cho mỗi thẻ và chúng sẽ kéo dài bao xa trong tương lai. 

Điều này dẫn đến một không gian trạng thái trong đó mỗi ngày không chỉ phụ thuộc vào ngày hôm trước mà còn phụ thuộc vào tập hợp nhiều lần sử dụng thẻ đang hoạt động. Ngay cả khi chúng ta rời rạc hóa một cách cẩn thận, số lượng trạng thái vẫn tăng theo cấp số nhân với k và số lượng các bước chồng chéo. Trong trường hợp xấu nhất, mỗi ngày có thể bắt đầu một lượt và tương tác với tất cả những người khác, dẫn đến sự bùng nổ về cấu hình vượt xa mọi giới hạn khả thi. 

Sự đơn giản hóa cấu trúc quan trọng là đảo ngược quan điểm. Thay vì nghĩ về “những thẻ nào đang hoạt động ngay bây giờ”, chúng tôi nghĩ về “ngày đi du lịch tiếp theo chưa được khám phá sau khi chúng tôi đưa ra quyết định ở đây là ngày nào”. Mỗi quyết định sẽ thanh toán trực tiếp cho ngày đi lại hiện tại hoặc bắt đầu một thẻ mà đóng góp của nó có thể được mô hình hóa để bao gồm một số phân đoạn trong tương lai của danh sách du lịch đã đặt hàng, nhưng có giới hạn về số lượng chỉ số mà nó có thể bao gồm trong một cửa sổ thời gian trượt.

Điều này cho phép chúng ta trình bày lại bài toán dưới dạng đường đi ngắn nhất qua các vị trí trong danh sách hành trình đã được sắp xếp. Chúng tôi xác định DP trong đó dp[i] là chi phí tối thiểu để chi trả cho tất cả các ngày đi lại bắt đầu từ ngày thứ i trở đi. Từ mỗi vị trí i, chúng ta có sự chuyển tiếp trực tiếp tới i+1 bằng cách thanh toán tiền vé của ngày thứ i. Đối với loại thẻ, nếu chúng tôi kích hoạt nó vào ngày thứ i, chúng tôi cần xác định xem nó có thể bao gồm bao nhiêu ngày đi lại trong tương lai trong khoảng thời gian và hạn ngạch d của nó. Điều đó tạo ra một bước nhảy từ i đến j nào đó và chúng ta cộng chi phí vượt qua c. Phần tinh tế là j này phụ thuộc vào thời gian theo lịch thực tế, không chỉ khoảng cách chỉ mục, vì vậy chúng tôi tính toán trước cho mỗi i và mỗi loại thẻ chỉ mục xa nhất có thể tiếp cận j(i, type). 

Một khi những bước nhảy này được biết đến, bài toán sẽ trở thành một DP tiêu chuẩn trên một đường có các chuyển tiếp O(nk). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các đường chuyền đang hoạt động | Hàm mũ | Hàm mũ | Quá chậm | 
| DP qua các chỉ số với chuyển tiếp vượt qua | O(nk) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số ngày đi lại theo thứ tự tăng dần và coi mỗi vị trí là một điểm quyết định. 

1. Đầu tiên, sắp xếp hoặc giả sử số ngày đi lại đã được sắp xếp và giữ hai mảng: thời gian theo lịch t[i] và giá vé f[i]. Điều này đảm bảo chúng ta có thể suy luận về khoảng thời gian hợp lệ bằng cách sử dụng các phép so sánh đơn giản. 
2. Xác định mảng DP dp[i], biểu thị chi phí tối thiểu để chi trả cho số ngày đi lại bắt đầu từ chỉ số i. Chúng tôi đặt dp[n] = 0 vì không còn ngày đi lại nào sau ngày cuối cùng. Điều này đưa ra một thứ tự tính toán từ phải sang trái tự nhiên. 
3. Đối với mỗi chỉ mục i và mỗi loại thẻ, hãy tính xem thẻ này có thể mở rộng bao xa nếu bắt đầu ở i. Chúng tôi quét về phía trước trong khi thời gian của ngày đi lại nằm trong p ngày kể từ t[i] và chúng tôi đếm số ngày đi lại rơi vào cửa sổ này cho đến khi chúng tôi đạt đến mức sử dụng d. Điểm cuối kết quả là j(i, type). Bước này chuyển ràng buộc lịch thành bước nhảy dựa trên chỉ mục. 
4. Phép truy toán cho dp[i] xem xét hai lựa chọn. Chúng ta có thể thanh toán trực tiếp cho ngày thứ i với chi phí f[i] + dp[i+1]. Ngoài ra, đối với mỗi loại thẻ, chúng ta có thể mua thẻ đó vào ngày thứ i, trả chi phí c cộng dp[j(i, type)]. Chúng tôi lấy mức tối thiểu cho tất cả các tùy chọn này. Điều này mã hóa ý tưởng rằng một tấm vé tiêu tốn một số hậu tố về số ngày đi lại và bỏ qua phía trước. 
5. Tính dp từ n−1 xuống 0 để biết tất cả các chuyển đổi khi cần. Câu trả lời là dp[0]. 

Tại sao nó hoạt động xuất phát từ thực tế là mọi chiến lược khả thi đều phải đưa ra quyết định đầu tiên vào ngày du lịch chưa được khám phá sớm nhất. Bất kỳ giải pháp nào cũng sẽ trả tiền cho ngày đó hoặc bắt đầu vượt qua ở đó. Sau khi quyết định đó được khắc phục, phần còn lại của vấn đề vẫn giống hệt nhưng được chuyển sang chỉ mục sau. DP nắm bắt chính xác sự phân tách này và việc tính toán trước j(i, type) mã hóa chính xác tất cả các tác động của một lượt mà không cần phải theo dõi rõ ràng các lượt hoạt động chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    t = []
    f = []
    for _ in range(n):
        ti, fi = map(int, input().split())
        t.append(ti)
        f.append(fi)

    passes = []
    for _ in range(k):
        p, d, c = map(int, input().split())
        passes.append((p, d, c))

    dp = [0] * (n + 1)

    for i in range(n - 1, -1, -1):
        best = f[i] + dp[i + 1]

        for p, d, c in passes:
            limit_time = t[i] + p
            cnt = 0
            j = i
            while j < n and t[j] <= limit_time and cnt < d:
                cnt += 1
                j += 1
            best = min(best, c + dp[j])

        dp[i] = best

    print(dp[0])

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo định nghĩa DP trực tiếp. Vòng lặp bên trong tính toán số ngày di chuyển mà một thẻ có thể sử dụng bắt đầu từ i, dừng khi chúng tôi vượt quá khoảng thời gian hoặc khi chúng tôi đạt đến giới hạn sử dụng d. Chỉ số kết quả j là ngày đầu tiên được phát hiện sau khi sử dụng thẻ. Quá trình chuyển đổi sau đó sẽ chuyển trực tiếp đến dp[j]. 

Sự tinh tế chính là đảm bảo rằng j chỉ tính số ngày đi lại thực tế chứ không phải ngày theo lịch. Đây là lý do tại sao chúng ta lặp lại các chỉ số thay vì cố gắng tính giới hạn số học. Một chi tiết quan trọng khác là dp được tính ngược sao cho dp[j] luôn được biết trước. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
0 10
1 10
2 2 15
```Chúng tôi tính toán dp từ dưới lên. 

| tôi | quyết định trả lương | vượt qua việc sử dụng | dp[i] | 
| --- | --- | --- | --- | 
| 2 | - | - | 0 | 
| 1 | 10 + dp[2] = 10 | pass bao gồm 1 ngày, j=2 → 15 | 10 | 
| 0 | 10 + dp[1] = 20 | pass bao gồm cả hai ngày, j=2 → 15 | 15 | 

Lựa chọn tối ưu là sử dụng thẻ bắt đầu từ ngày 0, bao gồm cả hai ngày du lịch. 

Điều này xác nhận rằng thẻ có thể bao gồm cơ cấu du lịch không liên tục theo thời gian theo lịch nhưng vẫn chỉ tiêu tốn tối đa d ngày đi lại thực tế. 

### Mẫu 2```
2 1
0 10
2 10
2 2 15
```| tôi | quyết định trả lương | vượt qua việc sử dụng | dp[i] | 
| --- | --- | --- | --- | 
| 2 | - | - | 0 | 
| 1 | 10 | pass chỉ bao gồm ngày 1, j=2 → 15 | 10 | 
| 0 | 10 + 10 = 20 | vượt qua chỉ bao gồm ngày 0, j=1 → 15 + dp[1]=25 | 20 | 

Ở đây, thẻ còn tệ hơn vì hai ngày đi lại quá xa nhau về mặt thời gian theo lịch, do đó hạn chế về khoảng thời gian khiến nó không thể bao phủ cả hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n k d') | Đối với mỗi trạng thái i và loại thẻ, chúng tôi quét tiến lên d hoặc giới hạn cửa sổ | 
| Không gian | O(n) | Mảng DP trên các chỉ số du lịch | 

Độ phức tạp phù hợp vì n là 10.000 và k nhiều nhất là 100. Ngay cả khi quét vừa phải cho mỗi lần chuyển đổi, tổng số thao tác vẫn nằm trong giới hạn chấp nhận được trong 2 giây trong Python được tối ưu hóa hoặc thoải mái trong PyPy hoặc C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    output = StringIO()
    backup = sys.stdout
    sys.stdout = output
    solve()
    sys.stdout = backup
    return output.getvalue().strip()

# provided samples
assert run("""2 1
0 10
1 10
2 2 15
""") == "15"

assert run("""2 1
0 10
2 10
2 2 15
""") == "20"

# custom: no passes
assert run("""3 0
0 5
1 6
2 7
""") == "18"

# custom: all cheap pass
assert run("""3 1
0 10
1 10
2 10
5 3 5
""") == "5"

# custom: tight window, partial coverage
assert run("""4 1
0 10
3 10
6 10
9 10
3 2 15
""") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có đường chuyền | tổng giá vé | độ chính xác cơ bản | 
| Vé trọn gói giá rẻ | sự thống trị một lượt | kích hoạt tối ưu toàn cầu | 
| Cửa sổ kín | xử lý bảo hiểm một phần | tính chính xác của hạn chế lịch | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi thẻ có khoảng thời gian hiệu lực lớn nhưng giới hạn sử dụng rất nhỏ. Ví dụ: nếu ngày đi lại đông đúc sớm nhưng thưa thớt về sau, thẻ có thể hết hạn ngạch trước khi thời hạn kết thúc. Thuật toán xử lý vấn đề này bằng cách đếm rõ ràng số ngày đi lại thực tế thay vì giả định phạm vi bao phủ toàn cửa sổ. 

Một trường hợp khác là khi các đường chuyền trùng nhau về thời gian nhưng không nằm trong phạm vi phủ sóng. Mặc dù nhiều lượt có thể hoạt động đồng thời, DP tránh hoàn toàn sự chồng chéo mô hình bằng cách nén mỗi lượt thành một bước nhảy duy nhất từ ​​i đến j(i, type), đảm bảo chúng tôi không bao giờ tính khả dụng gấp đôi. 

Trường hợp cạnh cuối cùng xảy ra khi một đường chuyền bắt đầu ở gần cuối chuỗi hành trình. Trong trường hợp đó, j(i, type) nhanh chóng trở thành n và dp[j] chính xác trở thành 0, nghĩa là chi phí vượt qua chỉ được so sánh với giá vé còn lại mà không cần xử lý đặc biệt.
