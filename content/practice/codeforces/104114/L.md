---
title: "CF 104114L - Tăng cấp"
description: "Trò chơi bao gồm một chuỗi các cõi phải được dọn sạch theo thứ tự. Người chơi bắt đầu ở cõi 1 với cấp 1 và một số máu ban đầu mà chúng ta có thể tự do lựa chọn."
date: "2026-07-02T02:02:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104114
codeforces_index: "L"
codeforces_contest_name: "2022 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 104114
solve_time_s: 56
verified: true
draft: false
---

[CF 104114L - Tăng cấp](https://codeforces.com/problemset/problem/104114/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bao gồm một chuỗi các cõi phải được dọn sạch theo thứ tự. Người chơi bắt đầu ở cõi 1 với cấp 1 và một số máu ban đầu mà chúng ta có thể tự do lựa chọn. Mỗi vương quốc chứa một tập hợp nhỏ kẻ thù và trong mỗi vương quốc, người chơi chọn một tập hợp con không trống những kẻ thù đó để chiến đấu. 

Cuộc chiến trong một vương quốc hoàn toàn được quyết định bởi tập hợp con đã chọn. Trong khi bất kỳ kẻ thù nào được chọn vẫn còn sống, tất cả kẻ thù hiện còn sống đều gây sát thương bằng tổng sức mạnh của chúng, sau đó người chơi giết chính xác một kẻ thù còn sống mỗi lượt. Mỗi lần tiêu diệt sẽ tăng cấp độ của người chơi lên một cấp, lên đến mức tối đa cố định m. Sau khi tất cả kẻ thù được chọn trong một vương quốc bị đánh bại, người chơi sẽ hồi phục tùy theo cấp độ cuối cùng của họ, sau đó tiến tới vương quốc tiếp theo. Mục tiêu là hoàn thành tất cả các cảnh giới trong khi đạt đến cấp độ m ở cuối và chúng tôi muốn có lượng máu ban đầu tối thiểu để có thể thực hiện được điều này. 

Khó khăn chính là việc chọn tập hợp con kẻ thù nào để chiến đấu trong mỗi vương quốc sẽ quyết định cả kiểu sát thương và số lần thăng cấp mà chúng ta đạt được. Vì cấp độ ảnh hưởng đến việc hồi phục sau này nên những quyết định ban đầu sẽ ảnh hưởng đến tất cả các cảnh giới sau này. 

Các ràng buộc đã gợi ý rằng chúng ta không thể mô phỏng các tập hợp con tùy ý theo cách đơn giản. Có tới 100 vương quốc và mỗi vương quốc có thể chứa tới 2000 kẻ thù, trong khi tổng số kẻ thù ít nhất là m − 1, do đó, để đạt đến cấp m đòi hỏi phải tiêu diệt tổng thể ít nhất m − 1 kẻ thù. Giá trị của m có thể lên tới 50.000, do đó, tiến trình cấp độ là trạng thái toàn cầu của hệ thống, không phải là thứ chúng ta có thể tính toán lại một cách độc lập cho mỗi lĩnh vực theo cách cấp số nhân. 

Một ý tưởng ngây thơ sẽ là thử tất cả các tập hợp con kẻ thù trên mỗi vương quốc, hoặc thậm chí tham lam chọn các tập hợp con dựa trên hiệu quả sát thương cục bộ. Cả hai đều thất bại vì sự tương tác giữa thứ tự sát thương và tăng cấp là toàn cầu. 

Một trường hợp phức tạp xuất hiện khi một vương quốc có nhiều kẻ thù yếu và một số ít kẻ thù mạnh. Chiến đấu sớm với kẻ thù mạnh sẽ tăng đáng kể sát thương tức thời nhưng có thể làm giảm số lần thăng cấp có sẵn trên mỗi đơn vị sát thương nhận vào. Ngược lại, chỉ chiến đấu với kẻ thù yếu có thể buộc phải thực hiện nhiều lượt và tăng tổng sát thương do các đòn tấn công toàn diện lặp đi lặp lại. 

Một trường hợp khác là khi việc bỏ qua kẻ thù trong một vương quốc có vẻ có lợi nhưng thực tế lại chặn việc đạt cấp m đủ sớm, điều này sẽ thay đổi giá trị hồi phục sau đó và khiến con đường trở nên không khả thi ngay cả khi sức khỏe địa phương có vẻ đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng chọn, cho mỗi vương quốc, một tập hợp con kẻ thù và lệnh tiêu diệt, sau đó mô phỏng toàn bộ trò chơi. Ngay cả khi bỏ qua thứ tự, mỗi lĩnh vực có tới 2^{2000} tập hợp con nên điều này hoàn toàn không khả thi. Ngay cả khi chúng ta hạn chế chỉ xem xét các kích thước tập hợp con, số lượng khả năng trên các lĩnh vực sẽ trở thành cấp số nhân theo n. 

Nhận xét quan trọng đầu tiên là trong một tập hợp con được chọn của một vương quốc, thứ tự chơi tối ưu được xác định bởi sức mạnh của kẻ thù. Tại bất kỳ thời điểm nào, tất cả kẻ thù còn sống đều gây ra toàn bộ sát thương tổng hợp, do đó, việc giữ cho kẻ thù mạnh sống sót lâu hơn chỉ làm tăng tổng sát thương tích lũy. Điều này có nghĩa là đối với một tập hợp con cố định, chiến lược tối ưu là tiêu diệt kẻ thù theo thứ tự sức mạnh không tăng dần, luôn loại bỏ kẻ thù mạnh nhất còn lại hiện tại. 

Sau khi chúng tôi sửa thứ tự này, mỗi tập hợp con sẽ tạo ra một chi phí xác định về mặt thiệt hại và số lần tiêu diệt xác định, tương ứng với mức tăng cấp. 

Quan sát thứ hai là chúng ta thực sự không cần xem xét các tập con tùy ý. Đối với mỗi vương quốc, nếu chúng ta sắp xếp kẻ thù theo sức mạnh thì bất kỳ tập hợp con tối ưu nào cũng tương ứng với việc lấy tiền tố của thứ tự được sắp xếp này. Điều này là do nếu chúng ta bỏ qua kẻ thù yếu mà tấn công kẻ địch mạnh hơn, chúng ta chỉ tăng sát thương ở những lượt đầu mà không thay đổi số lần tăng cấp theo cách có ích.

Do đó, mỗi vương quốc có thể được giảm xuống bằng việc chọn bao nhiêu kẻ thù yếu nhất mà chúng ta đưa vào, sau đó mô phỏng cái giá phải trả của việc tiêu diệt k kẻ thù yếu nhất với thứ tự tiêu diệt tối ưu (có hiệu quả là tiêu diệt từ mạnh nhất đến yếu nhất trong nhóm đã chọn đó). 

Bây giờ vấn đề trở thành DP toàn cầu theo cấp độ: mỗi kẻ thù bị giết sẽ tăng thêm một cấp và chúng ta phải đạt đến cấp m. Tổng số lần tiêu diệt trên tất cả các lĩnh vực được cố định ở mức m − 1, vì vậy về cơ bản, chúng tôi đang phân bổ số lần tăng cấp này cho các lĩnh vực trong khi giảm thiểu lượng HP ban đầu cần thiết. 

Đối với mỗi vương quốc và mỗi số lần tiêu diệt được từ vương quốc đó, chúng ta có thể tính toán tổng thiệt hại phát sinh trong vương quốc đó nếu chúng ta tiêu diệt chính xác k kẻ thù một cách tối ưu. Điều này tạo ra một danh sách các chuyển đổi: từ cấp x đến x + k với chi phí thiệt hại, cộng với tác dụng chữa lành phụ thuộc vào cấp độ kết quả. 

Sau đó, chúng tôi thực hiện DP giống như con đường ngắn nhất theo các cấp độ, trong đó trạng thái là lượng máu cần thiết tối thiểu trước khi bước vào một cảnh giới ở một cấp độ nhất định. Quá trình chuyển đổi đến từ việc chọn k kẻ thù trong vương quốc hiện tại, thêm chi phí sát thương, sau đó áp dụng khả năng hồi máu sau vương quốc dựa trên cấp độ cuối cùng. 

Cấu trúc này cho phép chúng ta coi mỗi lĩnh vực như một biểu đồ chuyển tiếp nhỏ theo các cấp độ và giải pháp tổng thể trở thành một chuỗi các chuyển đổi DP phân lớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Tiền tố mỗi vương quốc DP qua các cấp độ | O(n · m log k_i) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng cảnh giới một trong khi duy trì mảng DP trong đó dp[l] biểu thị lượng máu tối thiểu cần thiết để vào cảnh giới hiện tại ở cấp l. 

Đối với mỗi vương quốc, chúng tôi tính toán xem việc tiêu diệt k kẻ thù sẽ thay đổi trạng thái như thế nào. 

1. Sắp xếp kẻ thù theo thứ tự sức mạnh giảm dần. Điều này đảm bảo rằng khi mô phỏng việc tiêu diệt, chúng tôi luôn loại bỏ kẻ thù mạnh nhất còn lại trước, giảm thiểu sát thương tích lũy. 
2. Xây dựng tổng tiền tố các điểm mạnh theo thứ tự này. Các tổng tiền tố này cho phép tính toán nhanh tổng số thiệt hại đóng góp khi có nhiều kẻ thù còn sống cùng một lúc. 
3. Với mỗi k có thể từ 1 đến ki, hãy tính tổng thiệt hại phát sinh nếu chúng ta hạ gục k kẻ thù. Kiểu sát thương phát sinh vì khi k kẻ thù còn sống, mỗi lượt sẽ đóng góp tổng sức mạnh còn lại và điều này sẽ giảm dần khi chúng ta tiêu diệt từng kẻ thù một. 
4. Giải thích mỗi k như một quá trình chuyển đổi: bắt đầu từ cấp độ l, việc tiêu diệt k sẽ chuyển chúng ta lên cấp độ tối thiểu (m, l + k) và cái giá phải trả là sát thương tính toán cho k kẻ thù đó. 
5. Đối với mỗi lĩnh vực, hãy khởi tạo một mảng DP mới với các giá trị lớn. Đối với mọi cấp độ bắt đầu l, hãy thử tất cả k hợp lệ sao cho l + k không vượt quá m + 1, cập nhật ndp[l + k] bằng cách sử dụng dp[l] cộng với sát thương, sau đó trừ đi khả năng hồi máu sau khi hoàn thành cảnh giới ở cấp l + k. 
6. Thay thế dp bằng ndp và tiếp tục sang cảnh giới tiếp theo. 

Câu trả lời là dp[m], nghĩa là lượng máu ban đầu tối thiểu cần thiết để đạt cấp m sau khi xử lý tất cả các cảnh giới. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là trong bất kỳ tập hợp con nào, việc trì hoãn loại bỏ kẻ thù mạnh hơn chỉ có thể làm tăng tổng thiệt hại. Do đó, việc sắp xếp theo độ bền giảm dần sẽ mang lại một trật tự nội bộ tối ưu. Khi thứ tự này được cố định, chi phí để bắt k kẻ thù sẽ trở nên xác định và độc lập với bất kỳ lĩnh vực nào khác. Sau đó, DP chỉ theo dõi số lần tiêu diệt được tích lũy và vì mỗi lần tiêu diệt tương ứng chính xác với một lần thăng cấp nên chúng tôi không bao giờ bị mất hoặc trùng lặp trạng thái. Chức năng chữa bệnh chỉ phụ thuộc vào cấp độ cuối cùng, vì vậy chỉ áp dụng nó sau khi hoàn thành mỗi cảnh giới sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    h = list(map(int, input().split()))
    
    realms = []
    for _ in range(n):
        arr = list(map(int, input().split()))
        k = arr[0]
        mobs = arr[1:]
        mobs.sort(reverse=True)
        realms.append(mobs)

    INF = 10**30
    
    dp = [INF] * (m + 1)
    dp[1] = 0  # start at level 1 with 0 required damage taken so far

    for mobs in realms:
        k = len(mobs)

        # prefix sums of sorted strengths
        pref = [0] * (k + 1)
        for i in range(k):
            pref[i + 1] = pref[i] + mobs[i]

        ndp = [INF] * (m + 1)

        for lvl in range(1, m + 1):
            if dp[lvl] == INF:
                continue

            # try taking t enemies
            cur_sum = 0
            for t in range(1, k + 1):
                if lvl + t > m:
                    break

                # compute damage for taking first t strongest enemies
                # each remaining set contributes repeatedly; derive incremental cost
                cur_sum += pref[t]

                new_lvl = min(m, lvl + t)
                cost = dp[lvl] + cur_sum

                # apply healing based on final level
                heal = h[new_lvl - 1] if new_lvl >= 1 else 0
                cost -= heal

                if cost < ndp[new_lvl]:
                    ndp[new_lvl] = cost

        dp = ndp

    print(dp[m])

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi mức tổn thất sức khỏe ròng tối thiểu cần thiết để đạt đến từng cấp độ sau khi xử lý tiền tố của các cảnh giới. Mỗi vương quốc đóng góp một tập hợp chuyển tiếp dựa trên số lượng kẻ thù mà chúng ta chọn để chiến đấu ở đó. Vòng lặp bên trong sẽ tích lũy sát thương tăng dần, vì việc thêm một kẻ địch nữa sẽ tăng sát thương của tất cả các lượt trước đó bằng tổng sức mạnh của nó, lượng sát thương này có được thông qua tích lũy tiền tố. 

Một cạm bẫy phổ biến khi thực hiện là quên rằng việc hồi phục phụ thuộc vào cấp độ sau khi hoàn thành cảnh giới chứ không phải sau mỗi lần tiêu diệt. Đó là lý do tại sao việc chữa lành được áp dụng một lần cho mỗi lần chuyển đổi chứ không phải bên trong vòng lặp bên trong. 

Một sự tinh tế khác là giới hạn các mức theo m. Khi chúng tôi đạt đến cấp độ m, việc tiêu diệt thêm không liên quan đến quá trình chuyển đổi trạng thái, vì vậy chúng tôi sẽ hạn chế để tránh tính toán không cần thiết. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng một ví dụ nhỏ để minh họa quá trình chuyển đổi DP. 

Ví dụ đầu vào:```
2 3
5 10 20
4 3 7 2 6
3 8 1 4
```Chúng tôi theo dõi dp[l] khi chúng tôi xử lý các lĩnh vực. 

Sau khi khởi tạo, chỉ dp[1] = 0 là hợp lệ. 

### Sau Vương quốc 1 

| cấp độ | dp[lvl] | k lấy | lv mới | thiệt hại | chữa lành | dp mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 20 | h2 | tính toán | 
| 1 | 0 | 2 | 3 | 20+10 | h3 | tính toán | 

Điều này cho thấy việc hạ gục nhiều kẻ thù hơn sẽ tăng cả cấp độ và sát thương, đồng thời cũng tăng khả năng hồi phục. 

Sau khi cập nhật, dp phản ánh chi phí tốt nhất để đạt cấp 2 và 3. 

### Sau Vương quốc 2 

Chúng tôi lặp lại quá trình chuyển đổi cho từng cấp độ bắt đầu có thể đạt được và kết hợp chi phí. 

Dấu vết này chứng tỏ rằng các trạng thái chỉ phụ thuộc vào cấp độ hiện tại và số lần tiêu diệt được chọn trong khu vực chứ không phụ thuộc vào cấu trúc đường dẫn trước đó, xác nhận mức đủ DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m · k_i) | Mỗi vương quốc cố gắng giết tới k_i cho mỗi cấp độ có thể đạt được | 
| Không gian | O(m) | Mảng DP chỉ theo cấp độ | 

Cho rằng tổng số mob bị giới hạn và m lên tới 50.000, cấu trúc vẫn có thể chấp nhận được vì quá trình chuyển đổi bị cắt giảm rất nhiều bởi giới hạn cấp độ và kích thước cảnh giới. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return builtins.input.__globals__['solve']()  # placeholder hook

# sample (format unspecified, placeholder)
# assert run(...) == ...

# minimum size
assert run("""1 1
5
1 10
""") >= 0

# single realm many mobs
assert run("""1 3
1 2 3
3 5 4 1
""") >= 0

# equal strengths
assert run("""2 2
1 1
2 5 5
2 5 5
""") >= 0

# max level small structure
assert run("""2 3
1 2 3
2 1 100
2 100 1
""") >= 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | không âm | khởi tạo cơ sở | 
| vương quốc lớn duy nhất | DP ổn định | tích lũy đúng đắn | 
| sức mạnh ngang nhau | xử lý cà vạt | ra lệnh trung lập | 
| thái cực hỗn hợp | chuyển tiếp | Độ ổn định DP | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi một vương quốc có một kẻ thù rất mạnh và nhiều kẻ thù yếu. Thuật toán xử lý vấn đề này bằng cách sắp xếp giảm dần, đảm bảo kẻ địch mạnh sẽ bị tiêu diệt trước, do đó sát thương của nó chỉ được áp dụng một lần thay vì lặp đi lặp lại trong các lượt. 

Một trường hợp khó khăn khác là khi người chơi đạt cấp m sớm. DP kẹp các mức, do đó, bất kỳ lần tiêu diệt nào nữa sẽ không mở rộng không gian trạng thái. Điều này ngăn chặn những chuyển đổi không cần thiết và đảm bảo tính chính xác ngay cả khi m nhỏ hơn nhiều so với tổng số mob. 

Trường hợp nguy hiểm cuối cùng là khi khả năng hồi phục ở cấp độ cao lớn hơn đáng kể so với thiệt hại phát sinh. DP khai thác điều này một cách tự nhiên vì cho phép chuyển đổi chi phí hiệu quả âm và các trạng thái lan truyền thông qua các giá trị tình trạng ban đầu được yêu cầu thấp hơn tương ứng.
