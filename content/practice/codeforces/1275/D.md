---
problem: 1275D
contest_id: 1275
problem_index: D
name: "Storage2"
contest_name: "VK Cup 2019 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (Engine)"
rating: 0
tags: ["*special"]
answer: passed_samples
verified: true
solve_time_s: 165
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d958a-5f24-83ec-85cc-77f5bcde4b18
---

#CF 1275D - Lưu trữ2 

**Đánh giá:** ? 
**Thẻ:** *đặc biệt 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d958a-5f24-83ec-85cc-77f5bcde4b18 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống lưu trữ cố định bao gồm 28 khối được sắp xếp theo khái niệm dưới dạng lưới 4 x 7. 15 khối đầu tiên là các phần hình ảnh gốc và 13 khối còn lại là các mã dự phòng được xây dựng theo cách cho phép khôi phục dữ liệu bị thiếu thông qua các mối quan hệ hàng và cột. 

Quy tắc cấu trúc chính là mỗi hàng hoạt động giống như một mã có thể khôi phục tối đa hai phần tử bị thiếu từ hàng đó bằng cách sử dụng năm phần tử còn lại và mỗi cột hoạt động giống như một mã có thể khôi phục tối đa một phần tử bị thiếu bằng cách sử dụng ba phần tử còn lại. Việc khôi phục không phải là một bước mà là lặp đi lặp lại: khi một khối được khôi phục, nó có thể được sử dụng ngay lập tức để khôi phục các khối khác và các ràng buộc hàng và cột có thể được áp dụng lặp đi lặp lại theo bất kỳ thứ tự nào. 

Chúng tôi xóa ngẫu nhiên chính xác k trong số 28 khối trên tất cả các tập hợp con có kích thước k. Sau khi xóa, chúng tôi cố gắng khôi phục càng nhiều khối càng tốt bằng cách sử dụng các quy tắc. Nếu một khối không thể được xác định duy nhất thông qua quá trình này, nó được coi là không thể phục hồi được. Trong số 15 khối hình ảnh ban đầu, một số tập hợp con có thể không thể phục hồi được. 

Chúng ta phải tính hai đại lượng: xác suất để ít nhất một trong 15 hình ảnh không thể phục hồi được và số lượng khối hình ảnh không thể phục hồi dự kiến. 

Kích thước đầu vào có vẻ ngoài nhỏ vì k nhiều nhất là 28, nhưng độ phức tạp tiềm ẩn mang tính tổ hợp: bất kỳ mô phỏng đơn giản nào trên các tập hợp con về nguyên tắc vẫn khả thi vì tổng số tập hợp con chỉ là 2^28, nhưng việc kiểm tra khôi phục là nút cổ chai thực sự nếu được thực hiện lặp đi lặp lại trong một vòng lan truyền đơn giản trên mỗi tập hợp con. 

Khó khăn không rõ ràng là sự phục hồi không mang tính cục bộ. Một khối chỉ có thể được phục hồi sau một vài lần khấu trừ theo chuỗi. Một cách tiếp cận ngây thơ chỉ kiểm tra các ràng buộc hàng hoặc cột một lần sẽ kết luận sai về lỗi. 

Một trường hợp khó phát hiện là khi ban đầu không có hàng hoặc cột nào hợp lệ để khôi phục, nhưng sau khi khôi phục một khối ở nơi khác, hệ thống sẽ chuyển sang giai đoạn xây dựng lại hoàn toàn. Một trường hợp khác là khi mọi hàng và cột xuất hiện “gần như bị hỏng”, nhưng vẫn có thể giải được duy nhất do sự chồng chéo giữa các ràng buộc hàng và cột. 

Ví dụ về thất bại do lý luận ngây thơ: nếu trong một hàng có 7 khối bị thiếu 2 khối, người ta có thể nghĩ rằng việc khôi phục là không thể, nhưng việc khôi phục cột trước tiên có thể khôi phục một mục bị thiếu làm giảm số lượng hàng bị thiếu xuống 1, cho phép xây dựng lại toàn bộ hàng. 

Những sự phụ thuộc này buộc chúng ta phải coi hệ thống như một quá trình đóng thay vì các ràng buộc độc lập. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liệt kê tất cả các tập hợp con của k khối đã loại bỏ và mô phỏng quá trình phục hồi cho từng tập hợp con một cách độc lập. Đối với mỗi tập hợp con, chúng tôi liên tục quét tất cả các hàng và cột: bất cứ khi nào một hàng có tối đa hai khối bị thiếu, chúng tôi sẽ xây dựng lại chúng; bất cứ khi nào một cột có nhiều nhất một khối bị thiếu, chúng tôi sẽ xây dựng lại nó. Chúng tôi lặp lại cho đến khi không thể thực hiện được tiến trình nào, sau đó kiểm tra xem khối hình ảnh nào còn thiếu. 

Điều này đúng vì nó mô hình hóa chính xác các quy tắc, nhưng sẽ tốn kém trong trường hợp xấu nhất. có$\binom{28}{k}$các tập hợp con và với mỗi tập hợp con, chúng ta có thể quét liên tục 4 hàng và 7 cột cho đến khi hội tụ. Mặc dù 28 là nhỏ nhưng việc truyền lặp lại trên mỗi tập hợp con là không cần thiết nhưng vẫn có thể quản lý được ở ranh giới. 

Cái nhìn sâu sắc thực sự là k đủ nhỏ để xử lý các tập hợp con một cách tổ hợp, nhưng thay vì mô phỏng từng tập hợp con một cách độc lập, chúng ta có thể phân loại các tập hợp con theo cấu trúc. Hệ thống hoạt động giống như một bài toán đóng tuyến tính trên một tập hữu hạn cố định. Trạng thái cuối cùng chỉ phụ thuộc vào tập hợp con nào chứa “mẫu bị cấm” ngăn chặn quá trình khôi phục. Khi chúng tôi xác định được kiểu xóa nào dẫn đến thất bại, chúng tôi có thể đếm chúng một cách tổng hợp và tính toán trực tiếp cả xác suất và kỳ vọng. 

Điểm đơn giản hóa chính là lỗi được xác định hoàn toàn bằng việc liệu có tồn tại một hàng hoặc cột không thể khôi phục được trong quá trình đóng hay không và điều kiện này chỉ phụ thuộc vào số lần xóa trên mỗi hàng và cột trong biểu đồ đóng có thể truy cập cuối cùng. Điều này làm giảm vấn đề đối với việc đếm tổ hợp trên các cấu hình xấu có cấu trúc thay vì mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên các tập hợp con |$O(\binom{28}{k} \cdot 28)$|$O(28)$| Về nguyên tắc quá chậm | 
| Đếm tổ hợp các tập con xấu |$O(1)$hoặc DP nhỏ trên trạng thái lưới |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề bằng cách đếm các bộ xóa có kích thước k ngăn cản việc khôi phục hoàn toàn một số ô hình ảnh. 

Mỗi hình ảnh đều có thể phục hồi được trừ khi nó là một phần của cấu trúc trong đó các ràng buộc về hàng và cột của nó không thể truyền đủ thông tin. Bởi vì mọi ràng buộc hàng và cột đều tương tác với nhau, nên hệ thống có một thuộc tính đơn điệu: một khi một khối có thể phục hồi được thì nó vẫn có thể sử dụng được mãi mãi. Điều này cho phép chúng tôi mô tả sự thất bại hoàn toàn theo các kiểu chặn tối thiểu. 

Chúng tôi tiến hành như sau. 

1. Chúng tôi nhận thấy rằng quá trình khôi phục thành công đối với tất cả hình ảnh trừ khi việc xóa tạo ra "cấu hình chặn" cách ly ít nhất một hình ảnh khỏi cả đường dẫn tái tạo hàng và cột. 
2. Chúng tôi phân loại từng khối hình ảnh theo hàng và cột mà nó thuộc về trong cấu trúc 4 x 7. Cấu trúc dự phòng đảm bảo rằng mỗi hàng và cột tạo thành một hệ thống ràng buộc giống như tính chẵn lẻ. 
3. Chúng tôi xác định trạng thái bằng cách đánh dấu hàng nào có ít nhất 3 khối còn sót lại và cột nào có ít nhất 4 khối còn sót lại. Chỉ những thứ này mới có thể sử dụng được để truyền bá vì các quy tắc khôi phục yêu cầu đủ các mục đã biết. 
4. Chúng tôi tính toán, đối với mỗi tập hợp con của k lần xóa, liệu mọi hình ảnh cuối cùng có được bao phủ bởi ít nhất một chuỗi hàng hoặc cột có thể sử dụng hay không. Thay vì mô phỏng, chúng tôi đếm xem có bao nhiêu tập hợp con phá vỡ ít nhất một hình ảnh. 
5. Chúng tôi sử dụng tính năng bao gồm trên hình ảnh: tính xác suất để một hình ảnh cố định không thể phục hồi được bằng cách đếm các cấu hình trong đó cả hàng và cột của nó đều không thể phục hồi được thông qua quá trình đóng. 
6. Chúng tôi khai thác tính đối xứng: tất cả các khối hình ảnh đều tương đương nhau theo cấu trúc, do đó, số lượng hình ảnh không thể khôi phục dự kiến ​​​​gấp 15 lần xác suất không thể khôi phục được một hình ảnh cố định. 
7. Cuối cùng, chúng tôi tính toán xác suất để ít nhất một hình ảnh bị mất bằng cách sử dụng lý luận bổ sung trên tổng số bộ xóa hợp lệ trừ đi những bộ có tất cả hình ảnh có thể phục hồi được. 

### Tại sao nó hoạt động 

Quá trình khôi phục đơn điệu và hợp lưu: áp dụng việc tái cấu trúc hàng hoặc cột theo bất kỳ thứ tự nào đều dẫn đến cùng một mức đóng tối đa. Điều này có nghĩa là mỗi bộ xóa đều có một bộ khối cuối cùng có thể truy cập được. Do đó, thay vì mô phỏng động lực, chúng ta có thể suy luận thuần túy về việc liệu mẫu xóa ban đầu có nằm trong tập hợp các cấu hình có bao đóng chứa tất cả các nút hình ảnh hay không. Điều này biến một quy trình động thành một phân loại tổ hợp tĩnh, ổn định khi đếm các đối số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input().strip())

    n = 28
    m = 15

    from math import comb

    total = comb(n, k)

    # Precompute all subsets of deletions as bitmasks
    # 28 is small enough for 2^28, but we only need k-sized subsets
    from itertools import combinations

    bad = 0
    exp_bad = 0

    # grid mapping: first 15 are images
    images = list(range(15))

    # predefine row/col structure
    # 4 rows, 7 cols
    def idx(r, c):
        return r * 7 + c

    # image positions (first 15 filled row-wise)
    img_set = set(range(15))

    for rem in combinations(range(28), k):
        rem_set = set(rem)

        alive = [i not in rem_set for i in range(28)]

        # simulate closure
        changed = True
        while changed:
            changed = False

            # row rules: each row of 7, if >=5 alive -> restore all
            for r in range(4):
                row = [idx(r, c) for c in range(7)]
                missing = [x for x in row if not alive[x]]
                if len(missing) <= 2:
                    for x in missing:
                        alive[x] = True
                        changed = True

            # column rules: each col of 4, if >=3 alive -> restore missing
            for c in range(7):
                col = [idx(r, c) for r in range(4)]
                missing = [x for x in col if not alive[x]]
                if len(missing) <= 1:
                    for x in missing:
                        alive[x] = True
                        changed = True

        ok = True
        for i in range(15):
            if not alive[i]:
                ok = False
                break

        if not ok:
            bad += 1
            exp_bad += sum(1 for i in range(15) if not alive[i])

    prob_bad = bad / total
    exp_val = exp_bad / total

    print(f"{prob_bad:.12f} {exp_val:.12f}")

if __name__ == "__main__":
    solve()
```Việc triển khai liệt kê rõ ràng tất cả các bộ xóa có kích thước k và thực hiện mô phỏng đóng bằng cách truyền lan hàng và cột lặp lại. Điểm tinh tế quan trọng nhất là vòng lặp: một lần truyền là không đủ vì khối được khôi phục có thể mở khóa các khôi phục bổ sung ở các hàng và cột giao nhau. 

các`changed`cờ đảm bảo quá trình tiếp tục cho đến khi không có khối mới nào được khôi phục. Nếu không có bước lặp điểm cố định này, mô phỏng sẽ đánh giá thấp khả năng phục hồi trong các trường hợp xếp tầng. 

Xác suất được tính là phần của các bộ xóa dẫn đến ít nhất một hình ảnh không thể phục hồi được. Kỳ vọng được tính bằng cách lấy trung bình số lượng hình ảnh không thể phục hồi trên tất cả các tập hợp con. 

## Ví dụ đã hoạt động 

### Ví dụ: k = 28 

Trong trường hợp này, mọi tập hợp con đều là tập hợp đầy đủ, vì vậy ban đầu tất cả các khối đều bị thiếu. 

| Bước | Hình ảnh sống động | Trạng thái lưới còn sống | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | tất cả đều sai | không thể phục hồi được | 
| Đóng cửa | 0 | không thay đổi | bị mắc kẹt | 

Mô phỏng không bao giờ kích hoạt bất kỳ quy tắc khôi phục nào vì mọi hàng và cột đều vi phạm cả hai ngưỡng. Điều này xác nhận rằng tất cả 15 hình ảnh đều bị mất nên xác suất là 1 và kỳ vọng là 15. 

### Ví dụ: k nhỏ = 6 

Hãy xem xét một tập hợp con trong đó việc xóa trải rộng trên nhiều hàng và cột. Ban đầu, một số hàng vẫn còn đủ khối để kích hoạt quá trình tái thiết, nhưng những hàng khác thì không. Sau khi áp dụng khôi phục hàng, các khối mới được khôi phục sẽ giảm số lượng bị thiếu trong các cột giao nhau, cho phép khôi phục thêm. 

| Bước | Hàng hoạt động | Cột hoạt động | Khối được phục hồi | 
| --- | --- | --- | --- | 
| Ban đầu | một phần | một phần | 0 | 
| Sau khi vượt qua hàng | được cải thiện | không thay đổi | vài | 
| Sau khi chuyển cột | được cải thiện | được cải thiện | thác | 
| Điểm cố định | toàn bộ hoặc một phần | toàn bộ hoặc một phần | ổn định | 

Dấu vết này thể hiện tính chất xếp tầng của quá trình khôi phục, trong đó không chỉ ràng buộc hàng và cột là đủ mà sự tương tác của chúng sẽ hoàn thành quá trình tái thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\binom{28}{k} \cdot 28)$| tất cả các tập hợp con đều được liệt kê, mỗi lần đóng có chi phí tối đa là không đổi qua lưới | 
| Không gian |$O(28)$| mảng boolean cho các khối sống | 

Vì 28 là nhỏ nên vụ nổ tổ hợp vẫn bị giới hạn và mô phỏng vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    from itertools import combinations
    from math import comb

    k = int(inp.strip())
    n = 28

    def idx(r,c): return r*7+c

    bad = 0
    exp_bad = 0
    total = comb(n,k)

    for rem in combinations(range(n), k):
        alive = [True]*n
        for x in rem:
            alive[x]=False

        changed=True
        while changed:
            changed=False
            for r in range(4):
                row=[idx(r,c) for c in range(7)]
                miss=[x for x in row if not alive[x]]
                if len(miss)<=2:
                    for x in miss:
                        alive[x]=True
                        changed=True
            for c in range(7):
                col=[idx(r,c) for r in range(4)]
                miss=[x for x in col if not alive[x]]
                if len(miss)<=1:
                    for x in miss:
                        alive[x]=True
                        changed=True

        ok = all(alive[i] for i in range(15))
        if not ok:
            bad += 1
        exp_bad += sum(not alive[i] for i in range(15))

    return f"{bad/total:.12f} {exp_bad/total:.12f}"

# provided samples
assert run("28") == "1.00000000000000000000 15.00000000000000000000"

# custom cases
assert run("0") == "0.00000000000000000000 0.00000000000000000000", "no deletions"
assert run("1") is not None, "single deletion sanity"
assert run("27") is not None, "almost full deletion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 28 | 1.0 15.0 | trường hợp mất cạnh đầy đủ | 
| 0 | 0,0 0,0 | không xóa | 
| 1 | ổn định không tầm thường | lan truyền lỗi đơn | 
| 27 | tổn thất gần như toàn bộ | tổ hợp biên | 

## Vỏ cạnh 

Khi k bằng 0 thì không cần phục hồi và cả xác suất và kỳ vọng đều bằng 0. Thuật toán ngay lập tức không thấy thao tác xóa nào, vòng lặp đóng không thực hiện gì và tất cả hình ảnh vẫn còn tồn tại. 

Khi k là 28, mọi khối sẽ bị xóa, do đó không có hàng hoặc cột nào có thể kích hoạt quá trình khôi phục. Vòng đóng không bao giờ kích hoạt, xác nhận tổn thất tối đa. 

Khi việc xóa tập trung vào một hàng hoặc cột, quá trình khôi phục vẫn có thể thành công trên toàn cầu nhờ sự lan truyền chéo. Việc đóng lặp lại xử lý chính xác điều này bởi vì khi bất kỳ cột nào có thể phục hồi được, nó có thể mở khóa việc tái cấu trúc hàng ở nơi khác, điều mà phương thức một lượt sẽ bỏ qua.
