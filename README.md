# Giải mã mã nguồn của vắc-xin SARS-CoV-2 từ BioNTech/Pfizer


Xin chào! Trong bài này chúng ta sẽ phân tích chi tiết toàn bộ mã nguồn của vắc-xin mRNA SARS-CoV-2 tới từ BioNTech/Pfizer.

> Mình không phải tác giả mà dịch từ bản gốc tiếng anh [ở đây](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). Background của mình không phải là biotech nên trong quá trình dịch không thể tránh khỏi sai sót. Mọi đóng góp xin liên hệ với mình qua facebook [Ethan Phan](https://www.facebook.com/ethan.phan.79230/)

Điều này có thể nghe hơi lạ, vắc-xin là thứ chất lỏng để tiêm vào người thì tại sao chúng ta lại đan nói về mã nguồn ở đây?

Đây là một câu hỏi rất hay, hãy bắt đầu bằng việc nhìn vào một phần nhỏ của mã nguồn của vắc-xin từ BioNTech/Pfizer, được biết tới dưới cái tên BNT162b2, các tên gọi khác bao gồm Tozinameran hay Comirnaty.

<p align="center">
<img width="700" src="https://berthub.eu/articles/bnt162b2.png" alt="500 ký tư đầu tiên trong mã nguồn của BNT162b2">
</p>
<p align="center"> 500 ký tự đầu tiền trong mã nguồn của vắc-xin BNT162b2. Nguồn: <a href="https://mednet-communities.net/inn/db/media/docs/11889.doc">World Health Organization</a> </p>

Vắc-xin mRNA BNT162b có thành phần quan trọng nhất là một bộ mã nguồn kỹ thuật số dài 4284 ký tự, khá ngắn gọn để nằm vừa trong vài facebook post. Trong giai đoạn đầu tiền của quá trình sản xuất vắc-xin, đoạn mã nguồn này sẽ được đưa vào một máy in DNA (vâng, bạn không nghe nhầm đâu, máy in DNA là có thật đó), thứ sẽ dịch đoạn mã này thành những phân tử DNA thực sự.
<p align="center">
<img width="400" src="https://berthub.eu/articles/bioxp-3200.jpg" alt="Máy in DNA BioXp 3200">
</p>
<p align="center"> Máy in DNA BioXp 3200 đến từ <a href="https://codexdna.com/products/bioxp-system/">Codex DNA</a> </p>

Máy in sẽ tạo ra một lượng DNA rất nhỏ, thứ mà sau nhiều bước xử lý sinh học và hoá học sẽ biến thành RNA (sẽ nói thêm về RNA sau) trong các lọ vắc-xin. Một liều 30 microgram hoá ra chứa đúng 30 microgram RNA. Ngoài ra còn có một hệ thống đóng gói chất béo rất thông minh để đưa mRNA vào trong tế bào người.

RNA là phiên bản "bộ nhớ làm việc" dễ bay hơi (volatile ‘working memory’) của DNA. DNA giống như là chiếc ổ cứng sinh học vậy. DNA rất bền bỉ, dung lượng lớn đồng thời cũng rất tin cậy. Nhưng cũng giống như những chiếc máy tính không thể thực thi lệnh từ ổ cứng, trước khi được thực thi, lệnh phải được copy vào một bộ nhớ nhanh hơn, mềm dẻo hơn nhưng cũng mong manh hơn nhiều.

Trong máy tính, đó là RAM, trong sinh học là RNA. Chúng tương đồng một cách đáng kinh ngạc. Không giống như ổ cứng, dữ liệu trong RAM rất dễ biến mất nếu không được "chăm sóc" thường xuyên. Lý do mà vắc-xin mRNA của Pfizer/BioNTech phải được bảo quản ở nhiệt độ rất thấp cũng vậy: RNA là một đoá hoa mỏng manh.

Mỗi ký tự trong RNA nặng khoảng 0.53·10⁻²¹ gram, nghĩa là có khoảng 6·10¹⁶ ký tự trong một liều vắc-xin 30 microgram. Tính theo bytes thì là khoảng 25 petabytes, tuy nhiên nó vẫn chỉ là đoạn mã 4284 ký tự được copy tầm 2000 tỷ lần. Do đó lượng thông tin thực sự của vắc-xin chỉ khoảng hơn 1 kilobyte mà thôi. Bản thân vi rút SARS-CoV-2 chứa khoảng 7.5 kilobyte thông tin.

## Một chút thông tin thêm về DNA và RNA

DNA mà một đoạn mã kỹ thuật số. Không giống như trong những chiếc máy tính dùng 0 và 1, DNA dùng các ký tự A, C, G và U/T (các ‘nucleotides’, ‘nucleosides’ hay ‘bases).

A, C, G và U/T là các phân tử được lưu trữ dưới dạng chuỗi trong DNA (hoặc RNA).

Trong máy tính, mỗi nhóm 8 bít được nhóm lại thành 1 byte, và dữ liệu thường được xử lý theo từng byte.

Tự nhiên lại nhóm 3 nucleotides thành 1 condon (mình không biết tiếng việt cho mấy cái này), và condon thường là đơn vị xử lý cơ bản. Một condon chứa 6 bit dữ liệu (2 bit trên mỗi ký tự, 3 ký tự một condon => 6 bit. Đồng nghĩa với việc một codon có thể có có 2⁶ = 64 giá trị khác nhau).

Khá là kỹ thuật số phải không? Nếu còn thắc mắc về vấn đề này bạn có thể tra khảo thêm thông tin từ [tài liệu của WHO](https://mednet-communities.net/inn/db/media/docs/11889.doc).
> Một số thông tin chi tiết thêm có thể được tìm thấy [ở đây](https://berthub.eu/articles/posts/what-is-life/) - Đường link này ('Cuộc sống là gì') có thể giúp hiểu hơn về phần còn lại của bài này. Nếu bạn thích video, đây là [video dài 2 tiếng từ tác giả](https://berthub.eu/dna).

## Vậy cái đoạn mã đó nó làm gì?

Ý tưởng chung của vắc-xin là dạy cho hệ miễn dịch cách để chống lại các mầm bệnh mà không cần phải nhiễm các mầm bệnh đó. Việc này từ trước cho tới nay được thực hiện bằng cách tiêm vào trong cơ thể vi rút đã được làm yếu hoặc vô hiệu hoá (bất hoạt), cộng với một chất bổ trợ để kích thích hệ miễn dịch phản ứng. Cách này cần tới hàng tỷ quả trứng (hoặc côn trùng) để sản xuất ra vắc-xin. Cách này cũng tốn rất nhiều thời gian và đoi hỏi nhiều sự may mắn. Đôi khi cả một chủng vi rút không liên quan gì tới căn bệnh cũng được sử dụng.

Vắc-xin mRNA đạt được mục tiêu tương tự ('dạy cho hệ miễn dịch cách chống lại mầm bệnh') nhưng theo một cách như là tia laser vậy: rất tập trung nhưng cũng rất mạnh mẽ.

Và đây là cách nó hoạt động. Liều vắc-xin mà chúng ta đang nói tới chứa mRNA (hay messenger RNA) miêu tả phân tử protein 'Gai' (phân tử protein này nhìn như cái gai nằm trên mặt tế bào vi rút vậy) của vi rút SARS-CoV-2. Thông qua các phương tiện hoá học, vắc-xin sẽ đưa mRNA này vào trong tế bào người.

Điều này sau đó sẽ kích hoạt quá trình sản xuất protein 'Gai' của SARS-CoV-2 bên trong cơ thể người với một số đủ lớn để kích hoạt hệ thống miễn dịch phản ứng. Với sự xuất hiện của các protein Gai trong cơ thể cùng các dấu hiệu giống với việc các tế bào cơ thể bị nhiễm bệnh, hệ thống miễn dịch của chúng ta sẽ phát triển một cơ chế phòng vệ mạnh mẽ để vô hiệu hoá các protein Gai và đồng thời chống lại việc sản xuất các protein này.

Và đó là thứ giúp chúng ta đạt được thứ vắc-xin với hiệu quả miễn dịch 95%.

## Mã nguồn!

Hãy bắt đầu từ đầu, nơi rất tốt để bắt đầu. Tài liệu của WHO có hình ảnh rất trực quan như thế này:
<p align="center">
<img width="500" src="https://berthub.eu/articles/vaccine-toc.png" alt="Schematic">
</p>

Đây cũng giống như một cái mục lục vậy. Chúng ta sẽ bắt đầu với cái 'cap' (mũ), được miêu tả trong hình bằng cái mũ.

Cũng giống như việc bạn không thể quẳng opcodes trong một file đi và chaỵ nó vậy, hệ điều hành sinh học cũng cần headers, linkers và calling conventions.

Mã nguồn của vắc-xin đang nhắc tới bắt đầu với hai ký tự sau đây:
```
GA
```

Đây có thể so sánh với việc mọi file chạy trên Windows và DOS bắt đầu với `MZ`, hay mọi UNIX script đều bắt đầu với `#!` vậy. Trong cả hệ điều hành máy tính lẫn sinh học, các ký tự này đều không được thực thi. Nhưng chúng vẫn cần phải có mặt ở đó vì thiếu chúng thì sẽ không có gì xảy ra cả.

Cái 'mũ' mRNA [có một số chức năng nhất định](https://en.wikipedia.org/wiki/Five-prime_cap#Function). Đầu tiên, nó đánh dấu rằng đoạn mã đến từ nhân tế bào. Tuy nhiên trong trường hợp này thì đoạn mã không tới từ nhân tế bào mà tới từ vắc-xin. Nhưng chúng ta không cần phải nói với tế bào việc đó. Cái cap khiến cho đoạn mã nhìn giống như những đoạn mã khác trong cơ thể và tránh cho nó bị phá huỷ.

Hai phân tử nucleotide `GA` đầu tiên này cũng hơi khác phần còn lại của RNA về mặt hoá học. Về mặt này thì `GA` có thể được coi như một báo hiệu ngoài dải.

## Vùng không dịch năm phẩy ("5′ untranslated region" hay 5′ UTR)

Các phân tử RNA chỉ có thể được đọc theo một chiều. Cái phần mà việc đọc bắt đầu được gọi là 5' hay "five-prime" (trong tiếng việt có thể đọc là "năm-phẩy"). Việc đọc kết thúc ở đoạn gọi là 3' hay ba-phẩy. Việc đặt tên này khá là khó hiểu.

Sự sống bao gồm các protein (hoặc những thứ tạo nên bởi protein). Và các protein này được miêu tả trong RNA. Quá trình chuyển RNA thành các protein được gọi là dịch (translation).

Ở đây chúng ta có vùng không dịch 5' (vì nó không được dịch nên gọi là vùng không dịch), vậy nên đoạn mã này sẽ không xuất hiện trong phân tử protein:
```
GAAΨAAACΨAGΨAΨΨCΨΨCΨGGΨCCCCACAGACΨCAGAGAGAACCCGCCACC
```

Ở đây chúng ta có một sự ngạc nhiên đầu tiên. Bình thường RNA bao gồm các ký tự A, C, G và U. Ký tự U trong DNA được gọi là T. Nhưng ở đây lại có ký tự Ψ (đọc là 'sai'). Chuyện gì đang xảy ra vậy?

Đây là một phần rất thông minh về vắc-xin chúng ta đang nói tới. Cơ thể chúng ta có một hệ thống diệt vi rút rất mạnh mẽ. Vì lý do này các tế bào cực kỳ không hứng thú với RNA ngoại lai và rất cố gắng để phá huỷ nó trước khi nó kịp làm bất cứ điều gì.

Đây là một vấn đề với vắc-xin của chúng ta - nó cần vượt qua hệ miễn dịch. Qua nhiều năm nghiên cứu chúng ta đã phát hiện ra rằng nếu thay thế các phân tử U trong RNA bằng một phiên bản hơi khác của nó, hệ miễn dịch của chúng ta sẽ bỏ qua nó. Thật đấy.

Vậy nên trong vắc-xin của BioNTech/Pfizer, tất cả phân tử U được thay thế bằng phân tử 1-methyl-3’-pseudouridylyl, được ký hiệu bởi Ψ. Phần hết sức thông minh về việc này là ở chỗ mặc dù phân tử Ψ thay thế giúp RNA vượt qua hệ miễn dịch, nó vẫn được chấp nhận như phân tử U bình thường trong tế bào.

Trong bảo mật máy tính có một chiêu tương tự - đôi khi một gói tin đã bị làm méo có chủ đích có thể được gửi để đánh lừa firewall và các phần mềm bảo mật, nhưng nó vẫn được chấp nhận ở backend servers - thứ mà có thể bị hack bằng gói tin kia.

Chúng ta giờ đây đang gặt hái thành quả của các nghiên cứu khoa học nền tảng được thực hiện trong quá khứ. [Những nhà nghiên cứu](https://twitter.com/PennMedicine/status/1341766354232365059) khám phá ra kỹ thuật thay thế U bằng Ψ đã phải đấu tranh để giành được tiền tài trợ cho [nghiên cứu của họ](https://www.statnews.com/2020/11/10/the-story-of-mrna-how-a-once-dismissed-idea-became-a-leading-technology-in-the-covid-vaccine-race/). Tất cả chúng ta nên biết ơn vì điều đó, và tôi tin rằng [sớm thôi giải Nobel sẽ về tới tay họ](https://twitter.com/PowerDNS_Bert/status/1329861047168225281).

> Nhiều người hỏi rằng liệu vi rút có dùng kỹ thuật thay thế U bằng Ψ để vượt qua hệ miễn dịch không? Ngắn gọn là cực kỳ khó xảy ra. Sự sống đơn giản là không có phương tiện cần thiết để tạo ra phân tử 1-methyl-3’-pseudouridylyl. Các vắc-xin mRNA sẽ nhanh chóng bị tiêu biến trong cơ thể con người, và chẳng có cơ hội nào để các phân tử RNA với ký tự Ψ tồn tại trong đó cả. ["No, Really, mRNA Vaccines Are Not Going To Affect Your DNA"](https://www.deplatformdisease.com/blog/no-really-mrna-vaccines-are-not-going-to-affect-your-dna) là một bài rất đáng để đọc về vấn đề này.

Ok, quay lại với vùng không dịch 5'. 51 ký tự này có mục đích gì? Cũng giống trong tự nhiên, gần như chẳng cái gì có một chức năng rõ ràng cả.

Khi các tế bào người cần dịch RNA thành protein, việc này được thực hiện bởi một cơ quan tên là ribosome. Ribosome giống như là một cái máy in protein 3D vậy. Nó nhận đầu vào là một chuỗi RNA và dựa vào đó để sinh ra một sợi dây các amino acids, thứ mà sẽ gập lại thành một phân tử protein.


<p align="center">
<img width="400" src="https://github.com/EthanPhan/-Vietnamese-Reverse-Engineering-the-source-code-of-the-BioNTech-Pfizer-SARS-CoV-2-Vaccine/blob/main/Protein_translation.gif" alt="Protein tránlation">
</p>
<p align="center"> Nguồn: <a href="https://commons.wikimedia.org/wiki/File:Protein_translation.gif">Wikipedia user Bensaccount</a> </p>

Đây là những gì chúng ta thấy đang diễn ra phía trên. Sợi màu đen ở phía dưới là RNA. Còn sợi nằm bên trong vùng xanh lá là phân tử protein đang được hình thành. Những thứ đang bay ra bay vào là các amino a xít cộng với các adaptor để khiến chúng khớp với RNA.

Ribosome cần tiếp xúc trực tiếp với RNA để có thể làm việc. Một khi đã ngồi lên trên RNA, nó có thể bắt đầu việc sản sinh protein bằng việc di chuyển trên chuỗi RNA. Từ đây bạn có thể tưởng tượng rằng nó không thể đọc được phần RNA mà nó ngồi lên khi mới tiếp xúc với RNA. Đây chỉ là một trong các chức năng của vùng không dịch 5': chỗ hạ cánh cho ribosome.

Thêm vào đó, vùng không dịch còn cung cấp siêu dữ liệu (metadata): Khi nào thì việc dịch nên diễn ra? và bao nhiêu? Với vắc-xin, siêu dữ liệu này là 'ngay bây giờ' được lấy từ [gen alpha globin](https://www.tandfonline.com/doi/full/10.1080/15476286.2018.1450054). Gen này cũng được biết tới là sản xuất lượng protein rất lớn. Trong những năm trước đây, các nhà khoa học đã tìm ra cách tối ưu vùng không dịch này hơn nữa, vậy nên vùng không dịch trong vắc-xin thậm chí còn tốt hơn cả vùng không dịch của alpha globin.

## S glycoprotein signal peptide
> Thực sự mình không biết phải dịch cái đề mục này qua tiếng việt như thế nào.

Như đã lưu ý, mục tiêu của vắc-xin là khiến cho tế bào sản xuất một lượng lớn protein Gai của SARS-CoV-2. Cho tới hiện tại chúng ta mới hầu hết nói qua về siêu dữ liệu và những thứ ngoài lề. Bây giờ chúng ta sẽ đi vào địa hạt thực sự của protein của vi rút.

Tuy nhiên vẫn còn một lớp siêu dữ liệu nữa chúng ta phải đi qua. Một khi ribosome đã tạo ra một protein, protein đó cần phải đi đâu đó. Điều này được mã hoá trong "S glycoprotein signal peptide (chuỗi mở đầu mở rộng)". Mình dịch tạm là "peptide dẫn đường glycoprotein S" - thực sự là không nên dịch cái tên này.

Chuyện này có thể được nhìn theo hướng: ở đoạn đầu của protein có một dấu địa chỉ ngắn được mã hoá là một phần trong chính protein đó. Trong trường hợp cụ thể này, peptide dẫn đường nói rằng protein nên thoát ra khỏi tế bào thông qua một cơ quan có tên "endoplasmic reticulum". Ngay cả ngôn ngữ trong phim Star Trek cũng không fancy tới như thế này!

"Peptide dẫn đường" không quá dài, nhưng nếu chúng ta nhìn vào mã của nó sẽ thấy có vài điểm khác biệt giữa vắc-xin và vi rút:

(Để dễ so sánh, mình đã thay thế ký tư Ψ fancy bằng ký tự U thông thường)
```
           3   3   3   3   3   3   3   3   3   3   3   3   3   3   3   3
Virus:   AUG UUU GUU UUU CUU GUU UUA UUG CCA CUA GUC UCU AGU CAG UGU GUU
Vaccine: AUG UUC GUG UUC CUG GUG CUG CUG CCU CUG GUG UCC AGC CAG UGU GUU
               !   !   !   !   ! ! ! !     !   !   !   !   !            
```

Vậy chuyện gì đang diễn ra? Tôi đã viết đoạn mã theo nhóm 3 ký tự một cách tình cờ. Ba ký tự RNA làm thành một bộ ba (codon). Và mỗi bộ ba mã hoá một loại axit amin cụ thể. Peptide dẫn đường của vắc-xin chứa đúng những loại axit amin như trong bản thân vi rút.

Vậy tại sao mã RNA lại khác nhau?

Có 4³=64 bộ ba khác nhau, vì có 4 ký tự RNA khác nhau, và mỗi bộ ba chứa 3 ký tự. Tuy nhiên chỉ có 20 axit amin khác nhau mà thôi. Điều này có nghĩa rằng nhiều bộ ba khác nhau mã hoá cùng một axit amin.

Sự sống sử dụng bảng gần như là phổ quát dưới đây để ánh xạ từ các bộ ba thành các axit amin:
<p align="center">
<img width="700" src="https://berthub.eu/articles/rna-codon-table.png" alt="RNA codon table">
</p>
<p align="center"> <a href="https://en.wikipedia.org/wiki/DNA_and_RNA_codon_tables">Bảng mã hoá bộ ba RNA</a> (Wikipedia) </p>

Trong bảng này, chúng ta có thể thấy những thay đổi của vắc-xin (chẳng hạn UUU -> UUC) là *đồng nghĩa*. Mã RNA của vắc-xin khác, nhưng những axit amin và protein được tạo ra là giống với của vi rút.

Nếu nhìn kỹ hơn, chúng ta sẽ thấy đa số các thay đổi xảy ra ở vị trí thứ 3 trong của các bộ ba, đánh dấu bởi số 3 ở bên trên. Và nếu chúng ta kiểm tra bảng mã hoá bộ ba, chúng ta sẽ thấy rằng vị trí thứ 3 này thực ra thường không ảnh hưởng tới việc axit amin nào sẽ được sản xuất.

Vậy những thay đổi là *đồng nghĩa* thì tại sao chúng lại ở đó? Nhìn kỹ hơn chúng ta sẽ thấy tất cả các thay đổi ngoại trừ một vị trí sẽ dẫn tới có nhiều C hoặc G hơn trong chuỗi RNA.

Nhưng làm thế để làm gì? Như lưu ý ở trên, hệ miễn dịch của người có một cái nhìn khá lờ mờ về RNA ngoại lai, RNA tới từ bên ngoài tế bào. Để tránh bị phát hiện, các phân tử U đã được thay thế bằng Ψ.

Tuy nhiên, hoá ra RNA với lượng lớn phân tử G và C sẽ được chuyển hoá thành protein một cách hiệu quả hơn. Điều này đã đạt được bằng cách thay thế các phân tử bằng G và C nhiều nhất có thể.

> Cái biến đổi mà không làm tăng số lượng G hoặc C (CCA thành CCU) khiến tôi khá tò mò. Tôi sẽ rất muốn nghe lý do đằng say sự thay đổi bày. Tôi biết rằng một số bộ ba phổ biến trong gen người hơn các bộ ba khác, nhưng tôi cũng đọc được rằng [điều này không ảnh hưởng đáng kể tới tốc độ dịch](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006024).

## Protein Gai
3777 ký tự tiếp theo trong chuỗi RNA của vắc-xin cũng được tối ưu tương tự như vậy để tăng số lượng phân tử G và C. Để tiết kiệm diện tích tôi sẽ không liệt kê toàn bộ chuỗi ra đây mà sẽ tập trung vào một đoạn vô cùng đặc biệt trong chuỗi. Đoạn mã này là thứ khiến vắc-xin hoạt động, thứ giúp chúng ta đưa cuộc sống trở lại bình thường:
```
                   *   *
           L   D   K   V   E   A   E   V   Q   I   D   R   L   I   T   G
Vi rút:   CUU GAC AAA GUU GAG GCU GAA GUG CAA AUU GAU AGG UUG AUC ACA GGC
Vắc-xin:  CUG GAC CCU CCU GAG GCC GAG GUG CAG AUC GAC AGA CUG AUC ACA GGC
           L   D   P   P   E   A   E   V   Q   I   D   R   L   I   T   G
            !     !!! !!        !   !       !   !   !   ! !              
```

Ở đây chúng ta thấy các thay đổi *đồng nghĩa*. Ví dụ như ở bộ ba đầu tiên CUU được thay bằng CUG. Việc này thêm một phân tử G vào vắc-xin, điều mà chúng ta đã biết là giúp tế bào sản xuất protein hiệu quả hơn. Cả CUU và CUG đều là mã hoá của axit amin 'L' hay Leucine, vậy nên không có gì thay đổi ở protein cả.

Khi chúng ta so sánh toàn bộ protein Gai trong vắc-xin, tất cả thay đồi đều là *đồng nghĩa* như vậy ngoại trừ 2 vị trí, và đó là thứ chúng ta đang thấy ở đây.

Bộ ba thứ 3 và thứ 4 trong đoạn mã ở trên là các thay đổi thực sự. Các axit amin K và V đã được thay thế bằng 'P' hay Proline. Với axit amin K điều này cần 3 sự thay thế trong khi axit amin V chỉ cần 2.

__Hoá ra hai thay đổi này làm tăng mạnh mẽ hiệu quả của vắc-xin.__

Điều gì đang diễn ra ở đây vậy? Nếu nhìn vào các con vi rút SARS-CoV-2 thực tế bạn sẽ thấy các protein Gai là những chiếc gai trên bề mặt của vi rút.
<p align="center">
<img width="700" src="https://berthub.eu/articles/sars-em.jpg" alt="SARS virus particles">
</p>
<p align="center"> <a href="https://en.wikipedia.org/wiki/Severe_acute_respiratory_syndrome_coronavirus">Vi rút SARS</a> (Wikipedia) </p>

Những chiếc gai được gắn trên thân của vi rút (cũng là một phân tử protein có tên nucleocapsid). Vấn đề là vắc-xin của chúng ta lại không sinh ra cái gì để gài những chiếc gai protein lên cả.

Hoá ra, nếu không thay đổi gì, các gai protein tự do sẽ co lại thành một cấu trúc hoàn toàn khác. Nếu vắc-xin dùng các gai protein này cơ thể chúng ta sẽ sản sinh miễn dịch nhưng là miễn dịch chống lại các protein gai đã bị co lại mà thôi.

Và khi vi rút SARS-CoV-2 thực sự xuất hiện với những chiếc gai nhọn, hệ miễn dịch sẽ không hoạt động hiệu quả để chống lại chúng.

Vậy phải sao? Một [nghiên cứu năm 2017 đã mô tả việc thay thế hai axit amin Proline vào đúng vị trí](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5584442/) sẽ khiến cho các gai protein của vi rút SARS-CoV-1 và MERS không bị co lại mà giữ nguyên hình dáng ban đầu của nó dù không được gắn trên thân của vi rút. Điều này xảy ra là vì Proline là một axit amin rất 'cứng' (rigid). Nó giống như một chiếc nẹp giữ cho cấu trúc của protein ổn định ở trạng thái chúng ta cần.

[Những người](https://twitter.com/goodwish916) [đã khám phá ra](https://twitter.com/KizzyPhD) điều này nên đi vòng quanh và đập tay ăn mừng với nhau không ngừng. Họ có thể tỏ ra tự mãn một cách khủng khiếp. Họ xứng đáng được như vậy.

> Cập nhật! [McLellan lab](https://twitter.com/McLellan_Lab/status/1291077489566142464), một trong các nhóm đóng góp vào khám phá này, đã liên hệ với tôi. Họ nói rằng việc đập tay ăn mừng đã bị hạn chế lại bời vì cái đại dịch đang diễn ra, nhưng họ rất vui mừng vì đã có thể đóng góp vào loại vắc-xin chúng ta đang nói tới. Họ cũng nhấn mạnh sự đóng góp của nhiều những nghiên cứu khác, các nhân viên và cả các tình nguyện viên.

## Đoạn cuối của protein, những bước tiếp theo

Nếu đi qua một lượt phần còn lại của mã nguồn chúng ta sẽ gặp một vài sự sửa đổi ở cuối của protein Gai:
```
           V   L   K   G   V   K   L   H   Y   T   s             
Vi rút:   GUG CUC AAA GGA GUC AAA UUA CAU UAC ACA UAA
Vắc-xin:  GUG CUG AAG GGC GUG AAA CUG CAC UAC ACA UGA UGA 
           V   L   K   G   V   K   L   H   Y   T   s   s          
                !   !   !   !     ! !   !          ! 
```

Ở cuối của protein chúng ta sẽ thấy một bộ ba 'dừng', đánh dấu bởi ký tự 's' viết thường (stop). Đây là một cách lịch sự để thông báo rằng chuỗi protein nên kết thúc ở đây. Vi rút sử dụng bộ ba UAA để đánh dấu dừng trong khi vắc-xin dùng hai bộ ba UAG liền kề nhau.

## Vùng không dịch 3'

Cũng giống như việc ribosome cần một đoạn 'dẫn vào' ở vùng không dịch 5', ở cuối chuỗi RNA chúng ta sẽ thấy một vùng tương tự được gọi là vùng không dịch 3' (ba phẩy - three prime).

Có thể nói rất dài dòng về vùng không dịch 3', nhưng tôi sẽ trích một đoạn từ [Wikipedia](https://en.wikipedia.org/wiki/Three_prime_untranslated_region): "Vùng không dịch 3' đóng vai trò rất quan trọng vào việc biểu hiện gen bằng cách tác động tới sự định vị, ổn định, và hiệu quả của việc dịch của một mRNA.. __Với những hiểu biết hiện tại của chúng ta về những vùng không dịch 3', chúng vẫn là những ẩn số__".

Những gì chúng ta biết là một số vùng không dịch 3' nhất định rất hiệu quả trong việc giúp biểu hiện gen (khiến cho protein được sản xuất). Theo tài liệu từ WHO, vùng không dịch 3' của vắc-xin từ BioNTech/Pfizer
được lấy từ "the amino-terminal enhancer of split (AES) mRNA and the mitochondrial encoded 12S ribosomal RNA to confer RNA stability and high total protein expression" (Tôi xin phép không dịch đoạn này).

## Đoạn kết thúc tất cả AAAAAAAAAAAAAAAAAAAAAA

Đoạn cuối cùng của mRNA được thêm vào một cái đuôi poly(A). Đây một cách nói lịch sự khác của việc chuối mRNA kết thúc với một đống AAAAAAAAAAAAAAAAAAA. Có vẻ như ngay cả mRNA cũng đã chán ngấy năm 2020 rồi.

mRNA có thể được tái sử dụng nhiều lần, nhưng sau mỗi lần nó sẽ mất đi một vài phân tử A ở cuối. Một khi hết các phân tử A, mRAN sẽ ngường hoạt động và bị loại bỏ. Theo cách này, cái đuôi poly(A) là một cách bảo vệ mRNA chống lại sự thoái hoá.

Các nghiên cứu đã được thực hiện để tìm ra số lượng tối ưu của các phân tử A ở cuối chuỗi mRNA cho vắc-xin. Tôi đã đọc trong một tài liệu rằng con số này đạt đỉnh ở đâu đó 120 phân tử.

Vắc-xin BNT162b2 kết thúc với:
```
                                     ****** ****
UAGCAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAGCAUAU GACUAAAAAA AAAAAAAAAA 
AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAA
```
Đây là 30 ký tự A, sau đó là "10 nucleotide linker" (GCAUAUGACU), theo sau bởi 70 ký tự A.

Tôi đoán rằng những gì chúng ta thấy ở đây là kết quả của một cách tối ưu độc quyền để tăng hiệu quả của vắc-xin lên hơn nữa.
