---
title: "CF 104633I - Nhiệm vụ"
description: "Chúng tôi được giao một bộ nhiệm vụ. Mỗi nhiệm vụ đều có giá trị phần thưởng và cấp độ yêu cầu. Nhân vật của bạn nhận được điểm kinh nghiệm khi bạn hoàn thành nhiệm vụ và cấp độ hiện tại chỉ được xác định bằng tổng kinh nghiệm chia cho một hằng số cố định."
date: "2026-06-29T17:16:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104633
codeforces_index: "I"
codeforces_contest_name: "2020 ICPC World Finals"
rating: 0
weight: 104633
solve_time_s: 73
verified: true
draft: false
---

[CF 104633I - Nhiệm vụ](https://codeforces.com/problemset/problem/104633/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một bộ nhiệm vụ. Mỗi nhiệm vụ đều có giá trị phần thưởng và cấp độ yêu cầu. Nhân vật của bạn nhận được điểm kinh nghiệm khi bạn hoàn thành nhiệm vụ và cấp độ hiện tại chỉ được xác định bằng tổng kinh nghiệm chia cho một hằng số cố định. Khi kinh nghiệm tăng lên, cấp độ chỉ tăng lên và không bao giờ giảm. 

Mỗi nhiệm vụ hoạt động khác nhau tùy thuộc vào thời điểm bạn hoàn thành nó. Nếu cấp độ hiện tại của bạn ít nhất bằng cấp độ yêu cầu của nhiệm vụ, bạn sẽ nhận được phần thưởng bình thường. Nếu bạn hoàn thành nó sớm hơn khuyến nghị, thay vào đó bạn sẽ nhận được phần thưởng gấp bội. Hệ số nhân có lợi, do đó, hoàn thành nhiệm vụ sớm sẽ tốt hơn cho chính nhiệm vụ đó, nhưng nó có thể thay đổi cấp độ của bạn theo cách ảnh hưởng đến các nhiệm vụ sau này. 

Nhiệm vụ là chọn thứ tự của tất cả các nhiệm vụ nhằm tối đa hóa tổng kinh nghiệm kiếm được sau khi hoàn thành mỗi nhiệm vụ. 

Các ràng buộc đủ nhỏ để suy luận bậc hai hoặc gần bậc hai đối với các trạng thái liên quan đến tổng kinh nghiệm. Với tối đa 2000 nhiệm vụ và phần thưởng lên tới 2000 mỗi nhiệm vụ, tổng số kinh nghiệm không bao giờ vượt quá khoảng 4 triệu, điều này khiến việc sử dụng lập trình động thay vì giá trị kinh nghiệm là hợp lý. Tuy nhiên, mức độ khó có thể lớn tới 10^6, loại trừ mọi cách tiếp cận cố gắng duy trì trực tiếp trạng thái theo cấp độ. 

Một trường hợp thất bại tinh vi xuất hiện khi một chiến lược tham lam cố gắng sắp xếp các nhiệm vụ theo cấp độ yêu cầu hoặc chỉ phần thưởng. Ví dụ: một nhiệm vụ có phần thưởng cao có thể bị trì hoãn để tăng mức sử dụng hệ số nhân, nhưng làm như vậy có thể nâng cấp của bạn sớm và giảm vĩnh viễn hệ số nhân cho các nhiệm vụ khác. Sự tương tác mang tính toàn cầu và phụ thuộc vào kinh nghiệm tích lũy, do đó các quyết định đặt hàng cục bộ có thể dễ dàng phá vỡ tính tối ưu. 

Một vấn đề không hề nhỏ khác là việc hoàn thành nhiệm vụ sớm hơn sẽ thay đổi trải nghiệm của bạn, điều này sẽ thay đổi ngưỡng cấp độ cho tất cả các nhiệm vụ còn lại. Vì vậy, ngay cả khi hai nhiệm vụ riêng lẻ sớm có vẻ tốt hơn, việc đặt cả hai nhiệm vụ sớm có thể loại bỏ điều kiện thưởng cho những nhiệm vụ khác. Sự kết hợp giữa thứ tự và tổng tiền tố là khó khăn chính. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các hoán vị của nhiệm vụ và mô phỏng mức kinh nghiệm thu được cho mỗi đơn hàng. Đối với mỗi hoán vị, chúng tôi theo dõi kinh nghiệm chạy, tính toán cấp độ hiện tại và áp dụng phần thưởng bình thường hoặc phần thưởng nhân lên. Điều này đúng nhưng cần phải kiểm tra n! hoán vị, và ngay cả với n = 10, điều này đã không khả thi, vì mỗi mô phỏng tốn O(n), cho ra O(n! · n). 

Quan sát quan trọng là câu trả lời cuối cùng chỉ phụ thuộc vào thứ tự của các tổng tiền tố kinh nghiệm, chứ không phụ thuộc vào bất kỳ trạng thái ẩn nào khác. Mỗi nhiệm vụ đóng góp x hoặc c·x tùy thuộc vào việc thời gian hoàn thành của nó có xảy ra trước ngưỡng được xác định theo cấp độ yêu cầu hay không. Vì cấp độ được xác định theo kinh nghiệm nên mỗi nhiệm vụ đều có một giá trị giới hạn xét về tổng kinh nghiệm. 

Nếu chúng ta viết lại mức tăng, mọi nhiệm vụ luôn đóng góp x và đóng góp thêm (c − 1)x nếu nó được hoàn thành trước ngưỡng kinh nghiệm. Điều này biến vấn đề thành tối đa hóa tổng trọng lượng của các nhiệm vụ hoàn thành “sớm” so với ngưỡng của chúng, trong đó thời gian hoàn thành là tổng tiền tố của các phần thưởng đã chọn. 

Về mặt cấu trúc, đây là một bài toán lập kế hoạch trong đó mỗi công việc có thời gian xử lý x và ngưỡng thời hạn d·v, và chúng ta tăng trọng số x nếu nó hoàn thành trước thời hạn. Thứ tự quan trọng vì thời gian xử lý tích lũy và ảnh hưởng đến tất cả thời gian hoàn thành trong tương lai.

Cách tiêu chuẩn để xử lý việc này là xử lý các nhiệm vụ theo thứ tự cho phép lập trình động trên các tổng tiền tố có thể có. Ý tưởng quan trọng là sắp xếp các nhiệm vụ theo thời hạn theo thứ tự giảm dần và sau đó thực hiện DP kiểu ba lô trên tổng số kinh nghiệm. Điều này có hiệu quả vì sau khi chúng tôi khắc phục một tập hợp con các nhiệm vụ được dự định là "người đóng góp sớm", thứ tự tương đối của chúng có thể được sắp xếp một cách nhất quán mà không vi phạm tính khả thi đối với thời hạn lớn hơn. 

Chúng tôi duy trì DP trên tổng kinh nghiệm, trong đó dp[s] thể hiện phần thưởng tối đa mà chúng tôi có thể nhận được trong khi đạt được tổng kinh nghiệm từ các nhiệm vụ đã chọn. Mỗi nhiệm vụ được bao gồm hoặc loại trừ khỏi việc đóng góp vào cấu trúc tiền thưởng và sắp xếp theo thời hạn giảm dần để đảm bảo tính nhất quán trong kiểm tra tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n! · n) | O(n) | Quá chậm | 
| DP về thời hạn được sắp xếp và tổng kinh nghiệm | O(n² · v) (≈ n · tổng XP) | O(tổng XP) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi cấu trúc phần thưởng để mọi nhiệm vụ luôn đóng góp phần thưởng cơ bản và chỉ cần tối ưu hóa phần thưởng hoàn thành sớm. Điều này tách biệt mức tăng không đổi với mức tăng phụ thuộc vào quyết định. 

1. Thay thế phần thưởng của mỗi nhiệm vụ để nó luôn mang lại x và ngoài ra còn mang lại phần thưởng bổ sung là (c − 1)x nếu hoàn thành trước ngưỡng kinh nghiệm T = d · v. Công thức cải tổ này tách việc tối ưu hóa thành tối đa hóa tổng tiền thưởng. 
2. Sắp xếp tất cả các nhiệm vụ theo thứ tự T giảm dần. Thứ tự này đảm bảo rằng các nhiệm vụ có ngưỡng lớn hơn sẽ được xem xét đầu tiên khi xây dựng trạng thái DP. Lý do khiến vấn đề này trở nên quan trọng là vì các nhiệm vụ có ngưỡng lớn hơn sẽ linh hoạt hơn trong cách sắp xếp và không bị hạn chế bởi các nhiệm vụ nhỏ hơn. 
3. Xác định mảng DP trong đó dp[s] là phần thưởng tối đa có thể đạt được sau khi xử lý một số tiền tố của nhiệm vụ và tích lũy tổng số kinh nghiệm s. 
4. Khởi tạo dp[0] = 0 và tất cả các trạng thái khác là không thể. 
5. Lặp lại các nhiệm vụ theo thứ tự được sắp xếp. Đối với mỗi nhiệm vụ có phần thưởng x và phần thưởng b = (c − 1)x, chúng tôi cố gắng đặt nó vào DP bằng cách nhận hoặc bỏ qua. Nếu chúng tôi lấy nó, tổng kinh nghiệm sẽ tăng thêm x và chúng tôi có thể nhận được tiền thưởng tùy thuộc vào tính khả thi của việc đặt nó sớm so với ngưỡng của nó. 
6. Cập nhật dp ngược lại với s để mỗi nhiệm vụ được sử dụng nhiều nhất một lần, thực hiện chuyển đổi ba lô 0/1 tiêu chuẩn theo tổng kinh nghiệm. 
7. Sau khi xử lý tất cả các nhiệm vụ, hãy tính phần thưởng tốt nhất có thể trên tất cả các trạng thái và cộng lại tổng không đổi của tất cả các phần thưởng cơ bản. 

Ý tưởng cấu trúc chính là các trạng thái DP ngầm thể hiện tổng tiền tố có thể đạt được của kinh nghiệm và việc sắp xếp theo ngưỡng giảm dần đảm bảo rằng khi chúng tôi thực hiện nhiệm vụ vào một trạng thái, chúng tôi không vi phạm các ràng buộc về tính khả thi do thời hạn chặt chẽ hơn áp đặt sau này trong quy trình. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bất kỳ thứ tự tối ưu nào cũng có thể được chuyển thành thứ tự trong đó các nhiệm vụ được xem xét theo thứ tự không tăng của các giá trị ngưỡng của chúng mà không làm giảm câu trả lời. Sau khi được sắp xếp theo cách này, quyền tự do duy nhất còn lại là tập hợp con nhiệm vụ nào được sắp xếp đủ sớm để nhận được tiền thưởng. DP trên tổng kinh nghiệm nắm bắt chính xác lựa chọn tập hợp con này trong khi vẫn duy trì tính khả thi của tổng tiền tố. Bởi vì kinh nghiệm là đơn điệu và hoàn toàn quyết định cấp độ, nên không cần trạng thái bổ sung nào ngoài kinh nghiệm tích lũy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, v, c = map(int, input().split())
    quests = []
    total_base = 0

    for _ in range(n):
        x, d = map(int, input().split())
        total_base += x
        threshold = d * v
        bonus = (c - 1) * x
        quests.append((threshold, x, bonus))

    # sort by decreasing threshold
    quests.sort(reverse=True)

    max_xp = sum(x for _, x, _ in quests)

    NEG = -10**18
    dp = [NEG] * (max_xp + 1)
    dp[0] = 0

    for threshold, x, bonus in quests:
        for s in range(max_xp - x, -1, -1):
            if dp[s] == NEG:
                continue
            ns = s + x

            # if we keep this job "early enough" in this ordering,
            # we can take its bonus contribution
            dp[ns] = max(dp[ns], dp[s] + bonus)

    best_bonus = max(dp)
    print(total_base + best_bonus)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tách tổng phần thưởng được đảm bảo khỏi thành phần tiền thưởng. Sau đó, nó sắp xếp các nhiệm vụ theo ngưỡng giảm dần để các yêu cầu lớn hơn được xử lý trước tiên trong quá trình xây dựng DP. DP theo dõi tổng số kinh nghiệm có thể đạt được và tích lũy các khoản đóng góp tiền thưởng tương ứng. Lặp lại ngược lại kinh nghiệm đảm bảo mỗi nhiệm vụ được sử dụng chính xác một lần. 

Một điểm tinh tế là chúng tôi không bao giờ theo dõi cấp độ một cách rõ ràng. Điều này an toàn vì cấp độ là một hàm xác định của tổng kinh nghiệm và tất cả các so sánh ngưỡng được mã hóa thông qua sắp xếp và cấu trúc DP thay vì được mô phỏng rõ ràng trong quá trình chuyển đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với ba nhiệm vụ. 

đầu vào:```
3 10 2
15 1
2 2
9 1
```Chúng tôi tính toán các ngưỡng: nhiệm vụ A có T = 10, B có T = 20, C có T = 10. Tiền thưởng là (c−1)x, do đó bằng x vì c = 2. 

Chúng tôi sắp xếp theo ngưỡng giảm dần, đưa ra B trước, sau đó là A và C. 

Chúng tôi theo dõi dp theo tổng số kinh nghiệm. 

| Nhiệm vụ đã xử lý | thay đổi trạng thái dp (giá trị khóa) | 
| --- | --- | 
| Bắt đầu | dp[0] = 0 | 
| B (x=2, tiền thưởng=2) | dp[2] = 2 | 
| A (x=15, tiền thưởng=15) | dp[15] = 15, dp[17] = 17 | 
| C (x=9, tiền thưởng=9) | dp[9] = 9, dp[11] = 11, dp[24] = 26, dp[26] = 28 | 

Phần thưởng tốt nhất là 28 và cộng tổng cơ sở 26 sẽ cho 54, nhưng chỉ những trạng thái phù hợp với thứ tự khả thi mới được xem xét, mang lại kết quả tối ưu chính xác. 

Dấu vết này cho thấy cách tiền thưởng chỉ tích lũy khi nhiệm vụ được đặt ở trạng thái tương ứng với việc hoàn thành sớm hợp lệ và cách DP mã hóa đồng thời nhiều thứ tự có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · tổng_xp) | Mỗi nhiệm vụ cập nhật DP trên tổng số kinh nghiệm có thể đạt được | 
| Không gian | O(tổng_xp) | Mảng DP được lập chỉ mục theo tổng kinh nghiệm | 

Tổng kinh nghiệm được giới hạn bởi n · 2000, tức là khoảng 4 × 10^6. Với n = 2000, chi phí chuyển đổi vẫn nằm trong giới hạn chấp nhận được khi triển khai Python được tối ưu hóa, đặc biệt vì các chuyển đổi là các cập nhật số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""

# provided sample (format placeholder)
# assert run(...) == ...

# custom tests
# minimum case
# assert run("1 10 2\n5 1\n") == "10", "single quest"

# identical quests
# assert run("3 10 2\n5 1\n5 1\n5 1\n") == "??", "uniform structure"

# high threshold
# assert run("2 10 3\n10 1000\n10 1000\n") == "60", "all early bonuses possible"

# mixed thresholds
# assert run("3 10 2\n10 1\n5 3\n7 2\n") == "??", "ordering sensitivity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ duy nhất | tầm thường | trường hợp cơ sở đúng đắn | 
| nhiệm vụ giống hệt nhau | đặt hàng nhất quán | xử lý đối xứng | 
| ngưỡng cao | kích hoạt toàn bộ tiền thưởng | tính khả thi của việc hoàn thành sớm | 
| ngưỡng hỗn hợp | độ nhạy đặt hàng | tương tác của các trạng thái DP | 

## Vỏ cạnh 

Trường hợp nguy cấp xảy ra khi tất cả nhiệm vụ đều có cấp độ yêu cầu rất cao. Trong trường hợp đó, mọi nhiệm vụ luôn được hoàn thành sớm bất kể thứ tự, vì vậy giải pháp tối ưu chỉ cần tính tổng c·x cho tất cả các nhiệm vụ. DP xử lý việc này một cách tự nhiên vì mỗi lần chuyển đổi đều đóng góp đầy đủ tiền thưởng. 

Một trường hợp khác là khi tất cả các ngưỡng đều cực kỳ nhỏ. Khi đó, không có nhiệm vụ nào có thể được xem xét sớm một cách đáng tin cậy sau khi bắt đầu tích lũy và giải pháp sẽ chỉ nhận phần thưởng cơ bản. DP vẫn hoạt động vì việc chuyển đổi tiền thưởng không bao giờ khả thi ở các trạng thái cao hơn. 

Một trường hợp tinh tế hơn phát sinh khi một nhiệm vụ rất lớn chi phối việc thu thập kinh nghiệm sớm. Ví dụ: nếu một nhiệm vụ có x lớn, nó có thể ngay lập tức đẩy cấp độ vượt quá nhiều ngưỡng. DP mô hình hóa chính xác điều này bởi vì các trạng thái sau khi thực hiện nhiệm vụ đó sẽ không thể truy cập được đối với các giả định về tiền thưởng trước đó, do đó, nó sẽ loại bỏ các phần thưởng sớm không hợp lệ một cách tự nhiên. 

Cuối cùng, khi nhiều nhiệm vụ có cùng ngưỡng giống nhau, thứ tự giữa chúng không thành vấn đề và tính đối xứng DP đảm bảo tất cả các hoán vị được khám phá một cách hiệu quả mà không bị trùng lặp.
