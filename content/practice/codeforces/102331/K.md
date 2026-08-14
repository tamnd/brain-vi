---
title: "CF 102331K - Dây đàn K-pop"
description: "Chúng ta cần đếm các chuỗi có độ dài (n) trên một bảng chữ cái gồm 35 ký tự, cụ thể là các chữ số từ 1 đến 9 và các chữ cái viết thường. Một chuỗi hợp lệ nếu nó không chứa lặp lại song song có độ dài ít nhất là (n-k)."
date: "2026-08-13T03:55:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "K"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 262
verified: true
draft: false
---

[CF 102331K - Dây đàn K-pop](https://codeforces.com/problemset/problem/102331/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các chuỗi có độ dài (n) trên một bảng chữ cái gồm 35 ký tự, cụ thể là các chữ số`1`bởi vì`9`và các chữ cái viết thường. Một chuỗi hợp lệ nếu nó không chứa lặp lại song song có độ dài ít nhất là (n-k). 

Lặp lại song song là một chuỗi con có độ dài chẵn được tạo từ hai nửa liên tiếp giống hệt nhau. Ví dụ,`abab`là sự lặp lại song song vì hai nửa của nó đều là`ab`, trong khi`abca`thì không. Đầu ra là số chuỗi có độ dài-(n) tránh được mọi lần lặp lại song song đủ dài như vậy, modulo (998244353). 

Phần hữu ích của các ràng buộc là giá trị nhỏ của (k). Mặc dù (n) có thể là 100, nhưng mỗi lần lặp lại bị cấm đều có độ dài ít nhất là (n-k), do đó chỉ có một số lượng nhỏ các khoảng có thể bị cấm. Trên thực tế, số lần lặp lại song song ứng cử viên là (O(k^2)), nhiều nhất là vài chục cho các giới hạn đã cho. Điều này làm cho việc tìm kiếm loại trừ bao gồm đối với ứng cử viên lặp lại khả thi sau khi quan sát cắt tỉa mạnh mẽ hơn nhiều. 

Việc liệt kê đơn giản tất cả các chuỗi là hoàn toàn không thể. Với (n=100), có (35^{100}), khoảng (2,5\cdot10^{154}), chuỗi. Ngay cả khi việc kiểm tra một chuỗi chỉ mất (O(n^2)), tổng số sẽ là khoảng (10^{158}) thao tác. Kiểm tra trực tiếp mọi chuỗi con ứng cử viên và mọi so sánh ký tự gần với (O(35^n n^3)). 

Có một số trường hợp ranh giới trong đó việc triển khai có thể âm thầm sử dụng sai tập hợp các khoảng bị cấm. Vì`1 16`, không thể có bất kỳ lần lặp song song nào vì mỗi lần lặp song song có độ dài chẵn dương, trong khi chuỗi chỉ có một ký tự. Câu trả lời đúng là`35`. 

Vì`4 0`, chỉ lặp lại song song độ dài 4 đủ điều kiện. Điều kiện bị cấm duy nhất là (s_0=s_2) và (s_1=s_3), vì vậy câu trả lời là (35^4-35^2=1499400). Một giải pháp cấm lặp lại độ dài 2 ở đây sẽ là sai. 

Vì`2 16`, ngưỡng là (2-16=-14), do đó, mọi lần lặp lại song song đều đủ điều kiện. Việc lặp lại song song duy nhất có thể xảy ra là toàn bộ chuỗi, có nghĩa là hai ký tự không được bằng nhau. Câu trả lời là (35\cdot34=1190). Việc triển khai bất cẩn kẹp ngưỡng ở mức 2 hoặc giả sử (k<n) có thể xử lý sai trường hợp này. 

Vì`3 0`, ngưỡng là 3, nhưng các lần lặp song song phải có độ dài chẵn. Do đó, không có chuỗi con nào bị cấm và mọi chuỗi đều hợp lệ. Câu trả lời là (35^3=42875). Điều này bắt mã tự coi ngưỡng là độ dài lặp lại có thể mà không cần kiểm tra tính chẵn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo từng chuỗi trong số (35^n) và kiểm tra xem nó có chứa lặp lại song song bị cấm hay không. Đối với mỗi khoảng chẵn có độ dài ít nhất (n-k), hãy so sánh nửa đầu của nó với nửa sau của nó. Thử nghiệm này đúng vì định nghĩa của chuỗi K-pop chính xác là không có những khoảng đó. Vấn đề là số lượng chuỗi: tại (n=100), không gian tìm kiếm là khoảng (2,5\cdot10^{154}), do đó, ngay cả một thử nghiệm rẻ tiền phi thực tế cũng không thể làm cho phương pháp này trở nên hữu ích. 

Thay đổi quan trọng là ngừng liệt kê các chuỗi và thay vào đó liệt kê các mẫu bị cấm. Một sự lặp lại song song cụ thể không yêu cầu chúng ta phải biết chính các nhân vật. Nó chỉ áp đặt các ràng buộc bình đẳng giữa các vị trí. Ví dụ, sự kiện mà`abab`xảy ra ở vị trí (0\ldots3) áp đặt (s_0=s_2) và (s_1=s_3). 

Giả sử chúng ta chọn một số sự kiện lặp lại song song và yêu cầu tất cả chúng xảy ra đồng thời. Mọi ràng buộc đẳng thức đều kết nối hai vị trí chuỗi. Chúng ta có thể biểu diễn các ràng buộc bằng cấu trúc hợp tập hợp rời rạc. Nếu biểu đồ đẳng thức thu được có (c) các thành phần được kết nối thì mọi thành phần có thể chọn độc lập một trong 35 ký tự, do đó, chính xác các chuỗi (35^c) đáp ứng tất cả các sự kiện đã chọn. 

Điều này mang lại sự bao gồm-loại trừ. Nếu các sự kiện bị cấm là (E_1,E_2,\ldots,E_m), thì số chuỗi hợp lệ là 

[ 
35^n-\sum_i |E_i|+\sum_{i<j}|E_i\cap E_j|-\cdots. 
] 

Vấn đề rõ ràng là có thể có khoảng 80 sự kiện, vì vậy (2^m) tập hợp con là quá nhiều. Nhận xét quan trọng là nhiều nhánh của sự bao gồm-loại trừ này hoàn toàn dư thừa. Giả sử các sự kiện đã được chọn buộc mọi đẳng thức được yêu cầu bởi sự kiện hiện tại (E_i). Khi đó việc thêm (E_i) không làm thay đổi các thành phần được kết nối chút nào. Mọi giao lộ chứa (E_i) giống với giao lộ tương ứng không có (E_i), nhưng có dấu bao gồm-loại trừ ngược lại. Hai đóng góp đó bị hủy bỏ, bao gồm tất cả các lựa chọn có thể có của các sự kiện sau này. Toàn bộ chi nhánh có thể bị loại bỏ ngay lập tức. 

Chúng tôi duy trì các thành phần được kết nối bằng DSU khôi phục. Khi một sự kiện được thêm vào, chúng tôi ghi lại mọi kết hợp thành công, lặp lại và sau đó hoàn tác chính xác các kết hợp đó. Trước tiên, chúng tôi xử lý các lần lặp lại song song dài hơn. Thứ tự này làm cho các ràng buộc mạnh xuất hiện sớm và khiến các sự kiện về sau trở nên dư thừa thường xuyên hơn. Bài xã luận chính thức mô tả ý tưởng loại trừ và loại bỏ thành phần tương tự, đồng thời khuyến nghị giảm thời lượng lặp lại như một trong những mệnh lệnh hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(35^n n^3)) | (O(n)) | Quá chậm | 
| Loại trừ bao gồm với DSU khôi phục | (O(Bn\log n)) | (O(nk^2)) | Đã chấp nhận | 

Ở đây (B) là số nút đệ quy tồn tại sau quá trình cắt tỉa dự phòng. Việc tìm kiếm vẫn theo cấp số nhân trong trường hợp xấu nhất về mặt lý thuyết, nhưng toàn bộ vấn đề của việc xây dựng là (k\le16), chỉ có các sự kiện (O(k^2)) và hầu hết các nhánh loại trừ bao gồm đều chấm dứt ngay khi sự kiện hiện tại của chúng đã được ngụ ý. 

## Hướng dẫn thuật toán

1. Tạo mọi lần lặp lại song song bị cấm có thể. Đối với chiều dài chẵn (L) với (L\ge n-k), hãy chọn vị trí bắt đầu (l) của nó từ (0) đến (n-L). Nếu (h=L/2), sự kiện yêu cầu (s_{l+j}=s_{l+h+j}) cho mọi (0\le j<h). Chúng tôi lưu trữ các cặp vị trí đó thay vì chính chuỗi con. 
2. Sắp xếp các sự kiện theo độ dài giảm dần. Sự lặp lại dài hơn áp đặt nhiều ràng buộc bình đẳng hơn, do đó chúng có xu hướng làm cho các sự kiện sau này trở nên dư thừa. Thứ tự chính xác không phải là một phần của việc chứng minh tính chính xác nhưng nó có ảnh hưởng lớn đến kích thước thực tế của cây tìm kiếm. 
3. Khởi tạo DSU khôi phục với một thành phần cho mỗi vị trí chuỗi. Ban đầu có (n) thành phần, tương ứng với (35^n) chuỗi hoàn toàn không bị giới hạn. 
4. Xử lý các sự kiện theo cách đệ quy. Đầu tiên hãy lấy nhánh nơi sự kiện hiện tại không được chọn để loại trừ bao gồm. Sau đó tạm thời thêm tất cả các ràng buộc bình đẳng của sự kiện hiện tại vào DSU và đếm xem có bao nhiêu liên kết thực sự đã thay đổi phân vùng. 
5. Nếu việc thêm sự kiện không thực hiện kết hợp thành công thì sự kiện đã được các sự kiện được chọn ngụ ý. Các nhánh bao gồm và loại trừ của nó hủy bỏ chính xác, do đó trả về số 0 từ trạng thái đệ quy này mà không cần kiểm tra các sự kiện sau đó. 
6. Nếu sự kiện đưa ra ít nhất một đẳng thức mới, hãy lặp lại sự kiện đó. Dấu hiệu bao gồm-loại trừ của nó là âm so với nhánh nơi nó bị loại trừ, vì vậy hãy trừ kết quả của nhánh được bao gồm khỏi nhánh bị loại trừ. 
7. Khi tất cả các sự kiện đã được xử lý, DSU hiện tại chứa chính xác các giá trị bằng nhau được yêu cầu bởi tập hợp con các sự kiện đã chọn. Nếu nó có (c) các thành phần được kết nối thì có (35^c) chuỗi tương thích. Trả về giá trị đó. 
8. Sau mỗi nhánh được thêm vào, hãy đưa DSU trở lại ảnh chụp nhanh đã lưu. Điều này khôi phục chính xác phân vùng đã tồn tại trước khi sự kiện hiện tại được xem xét, do đó các nhánh đệ quy anh em không bao giờ ảnh hưởng lẫn nhau. 

Tại sao nó hoạt động: ở mọi trạng thái đệ quy, DSU thể hiện chính xác các đẳng thức được áp đặt bởi các sự kiện được chọn trên đường dẫn bao gồm-loại trừ hiện tại. Một thành phần được kết nối là một tập hợp các vị trí buộc phải mang cùng một ký tự và các thành phần khác nhau là độc lập, đưa ra các phép gán (35^c) tại một lá. Nếu sự kiện hiện tại không tạo ra sự hợp nhất thành phần mới thì ràng buộc của nó đã được các sự kiện đã chọn ngụ ý. Đối với mọi tập hợp con của các sự kiện sau này, giao điểm với sự kiện hiện tại sẽ giống với giao điểm không có sự kiện đó, trong khi hai dấu hiệu bao gồm-loại trừ thì ngược nhau. Đóng góp của họ bị hủy bỏ, do đó việc trả về số 0 là chính xác về mặt toán học. Mọi sự kiện không dư thừa đều được xem xét trong cả hai lựa chọn bao gồm-loại trừ có thể có, nghĩa là mọi tập hợp con đều được biểu thị bằng dấu đúng của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
ALPHABET = 35

def count_kpop(n, k):
    threshold = n - k

    events = []

    # An event is a forbidden tandem repeat.
    # For length L = 2*h starting at l, impose
    # s[l+j] == s[l+h+j] for 0 <= j < h.
    for L in range(max(2, threshold), n + 1):
        if L & 1:
            continue

        h = L // 2
        for l in range(n - L + 1):
            pairs = [(l + j, l + h + j) for j in range(h)]
            events.append((L, l, pairs))

    # Longer repeats first give much better pruning.
    events.sort(key=lambda x: (-x[0], x[1]))

    m = len(events)

    # powers[c] = 35^c mod MOD
    powers = [1] * (n + 1)
    for i in range(1, n + 1):
        powers[i] = powers[i - 1] * ALPHABET % MOD

    parent = list(range(n))
    size = [1] * n
    history = []
    components = n

    def find(x):
        while parent[x] != x:
            x = parent[x]
        return x

    def unite(a, b):
        nonlocal components

        a = find(a)
        b = find(b)

        if a == b:
            return False

        if size[a] < size[b]:
            a, b = b, a

        # Store enough information to undo this union.
        history.append((b, a, size[a]))

        parent[b] = a
        size[a] += size[b]
        components -= 1
        return True

    def rollback(snapshot):
        nonlocal components

        while len(history) > snapshot:
            b, a, old_size_a = history.pop()
            parent[b] = b
            size[a] = old_size_a
            components += 1

    sys.setrecursionlimit(1000000)

    def dfs(idx):
        # All selected events have now been fixed.
        if idx == m:
            return powers[components]

        # If all positions are already equal, every remaining event
        # is implied, so all inclusion-exclusion contributions cancel.
        if components == 1:
            return 0

        # Exclude the current event.
        result = dfs(idx + 1)

        # Include the current event.
        snapshot = len(history)
        changed = False

        for a, b in events[idx][2]:
            if unite(a, b):
                changed = True

        # The event was already implied by the current constraints.
        # Including or excluding it gives identical intersections,
        # so their contributions cancel completely.
        if not changed:
            rollback(snapshot)
            return 0

        result -= dfs(idx + 1)
        result %= MOD

        rollback(snapshot)
        return result

    return dfs(0)

def main():
    n, k = map(int, input().split())
    print(count_kpop(n, k))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`count_kpop`xây dựng chính xác các sự kiện quan trọng. Giới hạn dưới là`max(2, threshold)`bởi vì sự lặp lại song song có độ dài chẵn dương. Chúng tôi vẫn kiểm tra tính chẵn lẻ một cách riêng biệt vì khoảng lẻ không thể lặp lại song song. 

Mỗi sự kiện lưu trữ trực tiếp các cặp vị trí bằng nhau. Điều này tránh thực hiện bất kỳ thao tác chuỗi nào trong quá trình đệ quy. Để lặp lại độ dài (2h), có chính xác (h) ràng buộc đẳng thức. 

Mảng lũy ​​thừa được tính toán trước vì một lá của phép đệ quy chỉ cần số lượng thành phần DSU hiện tại. Tra cứu (35^c) khi đó là thời gian không đổi thay vì thực hiện phép lũy thừa mô-đun ở mỗi lá. 

DSU cố tình không sử dụng tính năng nén đường dẫn. Việc nén đường dẫn làm cho việc khôi phục trở nên phức tạp vì một`find`hoạt động có thể sửa đổi nhiều con trỏ cha. Ở đây, liên kết theo kích thước là đủ vì chỉ có (n\le100) vị trí và nó giữ cho mọi hoạt động tìm logarit. 

các`history`ngăn xếp chứa thông tin chính xác cần thiết để hoàn tác một liên minh thành công. Liên kết bị lỗi không sửa đổi DSU và do đó không cần phải ghi lại. 

Phần tinh tế nhất là`changed`Bài kiểm tra. Sẽ không đủ nếu hỏi liệu sự kiện có cặp đẳng thức nào hay không. Sự kiện nào cũng vậy. Điều quan trọng là liệu ít nhất một trong các đẳng thức của nó có kết nối được hai thành phần hiện tại khác nhau hay không. Nếu không có thì sự kiện sẽ không thêm thông tin mới và toàn bộ cây con loại trừ bao gồm của nó sẽ bị hủy. 

Phép trừ trong`result -= dfs(idx + 1)`là dấu bao gồm-loại trừ. Nhánh không có sự kiện đóng góp tích cực so với trạng thái hiện tại, trong khi nhánh có sự kiện đóng góp tiêu cực. Hoạt động modulo cuối cùng giữ kết quả trong phạm vi yêu cầu. 

Số nguyên Python không bị tràn nên mọi phép tính đều an toàn. Modulo được áp dụng cho kết quả đệ quy vì loại trừ bao gồm có thể tạo ra các giá trị trung gian âm và các giá trị dương rất lớn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`1 16`. Không có khoảng chẵn nào vừa với một chuỗi có độ dài bằng một, vì vậy danh sách sự kiện trống. 

| Chỉ mục sự kiện | Linh kiện | Hành động | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | Không có sự kiện nào tồn tại | (35^1=35) | 

Phép đệ quy ngay lập tức đến lá và trả về (35). Điều này thể hiện trường hợp ranh giới trong đó một chuỗi rất nhỏ không thể chứa bất kỳ sự lặp lại song song nào, bất kể giá trị của (k). 

Đối với Mẫu 2, đầu vào là`4 0`. Ngưỡng là 4, vì vậy sự kiện bị cấm duy nhất là toàn bộ chuỗi. Hai nửa của nó có độ dài 2, đưa ra các ràng buộc đẳng thức (s_0=s_2) và (s_1=s_3). 

| Chỉ mục sự kiện | Linh kiện | Hành động | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 4 | Loại trừ sự kiện | (35^4=1500625) | 
| 0 | 2 | Bao gồm sự kiện | (-35^2=-1225) | 
| 1 | 2 | Lá | (35^2=1225) | 

Giá trị cuối cùng là (1500625-1225=1499400). DSU có hai thành phần sau khi sự kiện được đưa vào vì vị trí 0 và 2 được gắn với nhau và vị trí 1 và 3 được gắn với nhau. Điều này khớp chính xác với chuỗi (35^2) chứa lặp lại song song có độ dài 4. 

Đối với Mẫu 3, đầu vào là`15 5`. Ngưỡng là 10, do đó chỉ các độ dài chẵn 10, 12 và 14 mới được xem xét. Mỗi vị trí bắt đầu có thể tạo ra một sự kiện, tạo ra một tập hợp nhỏ các mẫu bằng nhau dài. Thứ tự độ dài giảm dần cho phép DSU tích lũy các ràng buộc mạnh trước khi các sự kiện ngắn hơn được kiểm tra. 

| Độ dài lặp lại | Có thể bắt đầu | Cặp bình đẳng cho mỗi sự kiện | 
| --- | --- | --- | 
| 14 | 0, 1 | 7 | 
| 12 | 0, 1, 2, 3 | 6 | 
| 10 | 0, 1, 2, 3, 4, 5 | 5 | 

Phép đệ quy loại trừ bao gồm đánh giá các phép gán tương thích cho mọi giao lộ không dư thừa và hủy bỏ mọi nhánh dư thừa. Giá trị kết quả là`911125634`, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Bn\log n)) | (B) là số trạng thái đệ quy tồn tại sau quá trình cắt tỉa; xử lý một sự kiện sử dụng tối đa (O(n)) hoạt động DSU | 
| Không gian | (O(nk^2)) | Có các sự kiện (O(k^2)), mỗi sự kiện chứa các cặp đẳng thức (O(n)), cộng với rollback DSU | 

Tham số thực tế quan trọng là (B), không phải lý thuyết (2^m). Vì (k\le16), chỉ có (O(k^2)) ứng cử viên lặp lại và việc xử lý chúng từ dài nhất đến ngắn nhất khiến phần lớn các nhánh chấm dứt ngay khi sự kiện hiện tại đã được ngụ ý. Đây là cách tiếp cận dự định cho các giới hạn nhất định. Hướng dẫn chính thức mô tả rõ ràng việc cắt tỉa loại trừ bao gồm và thứ tự độ dài giảm dần hữu ích. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng`count_kpop`hoạt động từ giải pháp trên. Trình trợ giúp chuyển hướng đầu vào tiêu chuẩn để các xác nhận thực hiện định dạng đầu vào giống như chương trình đã gửi.```python
import sys
import io

MOD = 998244353

# Paste the solution's count_kpop function here,
# or import it from the submitted solution module.
from solution import count_kpop

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n, k = map(int, input().split())
        return str(count_kpop(n, k)) + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("1 16\n") == "35\n", "sample 1"
assert run("4 0\n") == "1499400\n", "sample 2"
assert run("15 5\n") == "911125634\n", "sample 3"

# Minimum size. No tandem repeat can exist.
assert run("1 0\n") == "35\n", "minimum size"

# All length-2 strings are tandem repeats exactly when both characters
# are equal. The answer is 35 * 34.
assert run("2 16\n") == "1190\n", "all-equal boundary"

# Odd n with k = 0. No even substring can reach length 3.
assert run("3 0\n") == "42875\n", "odd-length boundary"

# For n = 100, k = 0, only the complete length-100 string can be
# a forbidden tandem repeat.
expected = (pow(35, 100, MOD) - pow(35, 50, MOD)) % MOD
assert run("100 0\n") == f"{expected}\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`35`| Độ dài tối thiểu và tập sự kiện trống | 
|`2 16`|`1190`| Rất lớn (k), trong đó ngưỡng trở thành âm | 
|`3 0`|`42875`| Ngưỡng lẻ và yêu cầu độ dài song song là chẵn | 
|`100 0`| (35^{100}-35^{50}\pmod{998244353}) | Tối đa (n) và trường hợp sự kiện có độ dài đầy đủ duy nhất | 

## Vỏ cạnh 

cho`1 16`, vòng lặp tạo sự kiện bắt đầu ở độ dài 2, vốn đã lớn hơn (n). Danh sách sự kiện trống, do đó DSU bắt đầu với một thành phần và phép đệ quy trả về (35^1=35). Không có sự lặp lại nhân tạo về độ dài được giới thiệu. 

Vì`4 0`, ngưỡng chính xác là 4. Sự kiện được tạo duy nhất có độ dài 4 và hai cặp đẳng thức. Nhánh bao gồm giảm DSU từ bốn thành phần xuống còn hai, góp phần tạo ra (35^2) chuỗi xấu. Loại trừ bao gồm trừ những giá trị đó khỏi tất cả (35^4) chuỗi và đưa ra`1499400`. 

Vì`2 16`, ngưỡng là âm, nhưng việc triển khai không vô tình tạo ra các khoảng có độ dài bằng 0 hoặc một. Nó bắt đầu từ độ dài 2 và do đó tìm thấy chính xác một sự kiện, yêu cầu hai vị trí bằng nhau. Sự kiện được bao gồm để lại một thành phần DSU, do đó đóng góp của nó là (35) và kết quả là (35^2-35=1190). 

Vì`3 0`, ngưỡng là 3. Vòng lặp xem xét độ dài từ 3 đến 3, nhưng loại bỏ 3 vì nó là số lẻ. Không còn sự kiện nào nên tất cả (35^3=42875) chuỗi đều được tính. Đây là lý do tại sao việc kiểm tra tính chẵn lẻ trong quá trình xây dựng sự kiện là điều cần thiết. 

Đối với trường hợp kích thước tối đa`100 0`, ngưỡng là 100. Chỉ chuỗi hoàn chỉnh mới có thể bị cấm lặp lại song song. Hai nửa của nó có độ dài 50, do đó, các ràng buộc đẳng thức của nó để lại chính xác 50 lựa chọn ký tự độc lập. Kết quả là (35^{100}-35^{50}) modulo (998244353) và phép đệ quy chỉ chứa một sự kiện, khiến trường hợp này về cơ bản là trường hợp loại trừ bao gồm đơn giản nhất có thể mặc dù độ dài chuỗi tối đa.
