ifeq ($(CONFIG_SUPPORT_OPENWRT),y)
EXTRA_CFLAGS = -I$(src)/../src/include -I$(src)/../src/ate/include
DRV_NAME = mt7603e
SRC_DIR = ../src
obj-m += $(DRV_NAME).o
else
EXTRA_CFLAGS = -Idrivers/net/wireless/mt_wifi/include -Idrivers/net/wireless/mt_wifi/ate/include
DRV_NAME = mt_wifi
SRC_DIR = ../mt_wifi
obj-$(CONFIG_RT2860V2_AP) += $(DRV_NAME).o
endif

WIFI_HEAD_GEN  = $(shell make -C drivers/net/wireless/mt_wifi RT28xx_MODE=AP CHIPSET=mt7603e EE_TYPE=$(CONFIG_MT_PA_LNA) sdk_build_tools >/dev/null)

########################################################
# Common files
########################################################
cmm_objs := $(SRC_DIR)/common/crypt_md5.o\
						$(SRC_DIR)/common/crypt_sha2.o\
						$(SRC_DIR)/common/crypt_hmac.o\
						$(SRC_DIR)/common/crypt_aes.o\
						$(SRC_DIR)/common/crypt_arc4.o\
						$(SRC_DIR)/common/mlme.o\
						$(SRC_DIR)/common/cmm_wep.o\
						$(SRC_DIR)/common/action.o\
            $(SRC_DIR)/common/ba_action.o\
            $(SRC_DIR)/mgmt/mgmt_ht.o\
						$(SRC_DIR)/common/cmm_data.o\
						$(SRC_DIR)/common/rtmp_init.o\
						$(SRC_DIR)/common/rtmp_init_inf.o\
						$(SRC_DIR)/common/cmm_tkip.o\
						$(SRC_DIR)/common/cmm_aes.o\
						$(SRC_DIR)/common/cmm_sync.o\
						$(SRC_DIR)/common/eeprom.o\
						$(SRC_DIR)/common/cmm_sanity.o\
						$(SRC_DIR)/common/cmm_info.o\
						$(SRC_DIR)/common/cmm_cfg.o\
						$(SRC_DIR)/common/cmm_wpa.o\
						$(SRC_DIR)/common/cmm_radar.o\
						$(SRC_DIR)/common/spectrum.o\
						$(SRC_DIR)/common/rtmp_timer.o\
						$(SRC_DIR)/common/rt_channel.o\
						$(SRC_DIR)/common/rt_os_util.o\
						$(SRC_DIR)/common/cmm_profile.o\
						$(SRC_DIR)/common/scan.o\
						$(SRC_DIR)/common/cmm_cmd.o\
						$(SRC_DIR)/common/sys_log.o\
						$(SRC_DIR)/common/txpower.o\
						$(SRC_DIR)/chips/rtmp_chip.o\
						$(SRC_DIR)/mgmt/mgmt_hw.o\
						$(SRC_DIR)/mgmt/mgmt_entrytb.o\
						$(SRC_DIR)/tx_rx/wdev.o\
						$(SRC_DIR)/tx_rx/wdev_tx.o\
						$(SRC_DIR)/tx_rx/wdev_rx.o\
						$(SRC_DIR)/os/linux/rt_profile.o

########################################################
# Rate adaptation related files
########################################################
rate_objs := $(SRC_DIR)/rate_ctrl/ra_ctrl.o\
						$(SRC_DIR)/rate_ctrl/alg_legacy.o

ifeq ($(CONFIG_MT_NEW_RATE_ADAPT_SUPPORT),y)
    EXTRA_CFLAGS += -DNEW_RATE_ADAPT_SUPPORT
    rate_objs += $(SRC_DIR)/rate_ctrl/alg_grp.o

    ifeq ($(CONFIG_MT_AGS_SUPPORT),y)
        EXTRA_CFLAGS += -DAGS_ADAPT_SUPPORT
        rate_objs += $(SRC_DIR)/rate_ctrl/alg_ags.o
    endif
endif


########################################################
# ASIC related files
########################################################
asic_objs := $(SRC_DIR)/phy/phy.o

#ifeq ($(CONFIG_RLT_MAC),y)
#    EXTRA_CFLAGS += -DRLT_MAC
#
#    asic_objs += $(RT_WIFI_DIR)/mac/ral_nmac.o
#endif

#ifeq ($(CONFIG_RTMP_MAC),y)
#    EXTRA_CFLAGS += -DRTMP_MAC
#
#    asic_objs += $(RT_WIFI_DIR)/mac/ral_omac.o\
#		$(RT_WIFI_DIR)/chips/rtxx_dft.o
#endif


########################################################
# Spec feature related files
########################################################
spec_objs += $(SRC_DIR)/common/ps.o

ifeq ($(CONFIG_MT_UAPSD),y)
    EXTRA_CFLAGS += -DUAPSD_SUPPORT -DUAPSD_DEBUG
    spec_objs += $(SRC_DIR)/common/uapsd.o
endif

#ifeq ($(CONFIG_RLT_MAC),y)
#	spec_objs += $(RT_WIFI_DIR)/common/ral_ps.o
#endif

#ifeq ($(CONFIG_RTMP_MAC),y)
#	spec_objs += $(RT_WIFI_DIR)/common/ral_ps.o
#endif

ifeq ($(CONFIG_MT_MAC),y)
	EXTRA_CFLAGS += -DMT_PS
	spec_objs += $(SRC_DIR)/common/mt_ps.o
	spec_objs += $(SRC_DIR)/common/mt_io.o
	spec_objs += $(SRC_DIR)/tx_rx/txs.o
endif

# WSC
ifeq ($(CONFIG_MT_WSC_INCLUDED),y)
    EXTRA_CFLAGS += -DWSC_INCLUDED -DWSC_SINGLE_TRIGGER

    ifneq ($(CONFIG_MT_AP_SUPPORT),)
        EXTRA_CFLAGS += -DWSC_AP_SUPPORT
    endif

    ifneq ($(CONFIG_MT_STA_SUPPORT),)
        EXTRA_CFLAGS += -DWSC_STA_SUPPORT
    endif

    spec_objs += $(SRC_DIR)/common/wsc.o\
                    $(SRC_DIR)/common/wsc_tlv.o\
                    $(SRC_DIR)/common/crypt_dh.o\
                    $(SRC_DIR)/common/crypt_biginteger.o\
                    $(SRC_DIR)/common/wsc_ufd.o

    ifeq ($(CONFIG_MT_WSC_V2_SUPPORT),y)
        EXTRA_CFLAGS += -DWSC_V2_SUPPORT
        spec_objs += $(SRC_DIR)/common/wsc_v2.o
    endif
endif

# VHT
ifeq ($(CONFIG_MT_DOT11_VHT_AC),y)
    EXTRA_CFLAGS += -DDOT11_VHT_AC
    spec_objs += $(SRC_DIR)/mgmt/mgmt_vht.o\
            $(SRC_DIR)/common/vht.o
endif

ifeq ($(CONFIG_MT_SNIFFER_SUPPORT),y)
    EXTRA_CFLAGS += -DCONFIG_SNIFFER_SUPPORT
    spec_objs += $(SRC_DIR)/sniffer/sniffer_radiotap.o\
            $(SRC_DIR)/sniffer/sniffer_prism.o
endif

ifeq ($(CONFIG_MT_ACL_V2_SUPPORT),y)
    EXTRA_CFLAGS += -DACL_V2_SUPPORT
    spec_objs += $(SRC_DIR)/ap/ap_acl_v2.o
endif

# WAPI
ifeq ($(CONFIG_MT_WAPI_SUPPORT),y)
    EXTRA_CFLAGS += -DWAPI_SUPPORT
#    ifeq ($(CONFIG_RALINK_RT3052),y)
#        EXTRA_CFLAGS += -DSOFT_ENCRYPT
#    endif

  spec_objs += $(SRC_DIR)/common/wapi.o\
                $(SRC_DIR)/common/wapi_sms4.o\
		$(SRC_DIR)/common/wapi_crypt.o
		
endif

# ACM
ifeq ($(CONFIG_MT_WMM_ACM_SUPPORT),y)
    EXTRA_CFLAGS += -DWMM_ACM_SUPPORT

    spec_objs += $(SRC_DIR)/common/acm_edca.o\
            $(SRC_DIR)/common/acm_comm.o\
            $(SRC_DIR)/common/acm_iocl.o
endif

#PMF
ifeq ($(CONFIG_MT_DOT11W_PMF_SUPPORT),y)
    EXTRA_CFLAGS += -DDOT11W_PMF_SUPPORT -DSOFT_ENCRYPT

    spec_objs += $(SRC_DIR)/common/pmf.o
endif

# 11N BSS CoEx
ifeq ($(CONFIG_MT_DOT11N_DRAFT3),y)
    EXTRA_CFLAGS += -DDOT11N_DRAFT3
endif

# LLTD
ifeq ($(CONFIG_MT_LLTD_SUPPORT),y)
    EXTRA_CFLAGS += -DLLTD_SUPPORT
endif

# FT
#ifeq ($(CONFIG_RT2860V2_80211R_FT),y)
#dot11_ft_objs += $(SRC_DIR)/common/ft.o\
#                    $(SRC_DIR)/common/ft_tlv.o\
#                    $(SRC_DIR)/common/ft_ioctl.o\
#                    $(SRC_DIR)/common/ft_rc.o\
#                    $(SRC_DIR)/ap/ap_ftkd.o
#endif

# RR
#ifeq ($(CONFIG_RT2860V2_80211K_RR),y)
#dot11k_rr_objs += $(SRC_DIR)/common/rrm_tlv.o\
#                    $(SRC_DIR)/common/rrm_sanity.o\
#                    $(SRC_DIR)/common/rrm.o
#endif

#HOTSPOT
ifeq ($(CONFIG_MT_HOTSPOT),y)
    EXTRA_CFLAGS += -DCONFIG_DOT11U_INTERWORKING -DCONFIG_DOT11V_WNM -DCONFIG_HOTSPOT

    spec_objs += $(SRC_DIR)/common/wnm.o\
				$(SRC_DIR)/common/gas.o\
				$(SRC_DIR)/common/hotspot.o
ifeq ($(CONFIG_MT_HOTSPOT_R2),y)
EXTRA_CFLAGS += -DCONFIG_HOTSPOT_R2
endif
endif

#
# Common Feature related files
#
func_objs :=

ifeq ($(CONFIG_MT_IGMP_SNOOP_SUPPORT),y)
    EXTRA_CFLAGS += -DIGMP_SNOOP_SUPPORT

    func_objs += $(SRC_DIR)/common/igmp_snoop.o
endif

ifeq ($(CONFIG_MT_BLOCK_NET_IF),y)
    EXTRA_CFLAGS += -DBLOCK_NET_IF

    func_objs += $(SRC_DIR)/common/netif_block.o
endif

ifeq ($(CONFIG_MT_SINGLE_SKU_V2),y)
    EXTRA_CFLAGS += -DSINGLE_SKU_V2
endif

ifeq ($(CONFIG_RT2860V2_AP_VIDEO_TURBINE),y)
    EXTRA_CFLAGS += -DVIDEO_TURBINE_SUPPORT

    func_objs += $(SRC_DIR)/common/cmm_video.o
endif


ifeq ($(CONFIG_MT_LED_CONTROL_SUPPORT),y)
    EXTRA_CFLAGS += -DLED_CONTROL_SUPPORT
    ifeq ($(CONFIG_MT_WSC_INCLUDED),y)
        EXTRA_CFLAGS += -DWSC_LED_SUPPORT
    endif

    func_objs += $(SRC_DIR)/common/rt_led.o
endif


########################################################
# AP feature related files
########################################################
ap_objs := $(SRC_DIR)/ap/ap.o\
            $(SRC_DIR)/ap/ap_assoc.o\
            $(SRC_DIR)/ap/ap_auth.o\
            $(SRC_DIR)/ap/ap_connect.o\
            $(SRC_DIR)/ap/ap_mlme.o\
            $(SRC_DIR)/ap/ap_sanity.o\
            $(SRC_DIR)/ap/ap_sync.o\
            $(SRC_DIR)/ap/ap_wpa.o\
            $(SRC_DIR)/ap/ap_data.o\
            $(SRC_DIR)/ap/ap_autoChSel.o\
            $(SRC_DIR)/ap/ap_qload.o\
            $(SRC_DIR)/ap/ap_cfg.o\
            $(SRC_DIR)/ap/ap_nps.o\
            $(SRC_DIR)/os/linux/ap_ioctl.o            

ifeq ($(CONFIG_MT_QOS_DLS_SUPPORT),y)
    EXTRA_CFLAGS += -DQOS_DLS_SUPPORT
    ap_objs += $(SRC_DIR)/ap/ap_dls.o
endif

ifeq ($(CONFIG_MT_MBSS_SUPPORT),y)
    EXTRA_CFLAGS += -DMBSS_SUPPORT

    ifeq ($(CONFIG_MT_NEW_MBSSID_MODE),y)
        EXTRA_CFLAGS += -DNEW_MBSSID_MODE
        ifeq ($(CONFIG_MT_ENHANCE_NEW_MBSSID_MODE),y)
            EXTRA_CFLAGS += -DENHANCE_NEW_MBSSID_MODE
        endif
    endif

    ap_objs += $(SRC_DIR)/ap/ap_mbss.o\
            $(SRC_DIR)/ap/ap_mbss_inf.o
endif


ifeq ($(CONFIG_MT_WDS_SUPPORT),y)
    EXTRA_CFLAGS += -DWDS_SUPPORT

    ap_objs += $(SRC_DIR)/ap/ap_wds.o\
            $(SRC_DIR)/ap/ap_wds_inf.o\
            $(SRC_DIR)/common/client_wds.o
endif

ifeq ($(CONFIG_MT_APCLI_SUPPORT),y)
    EXTRA_CFLAGS += -DAPCLI_SUPPORT -DMAT_SUPPORT
#-DAPCLI_AUTO_CONNECT_SUPPORT
    ifeq ($(CONFIG_RALINK_MT7620),y)
        EXTRA_CFLAGS += -DMAC_APCLI_SUPPORT
    endif

    ifeq ($(CONFIG_MT_MULTI_APCLI_SUPPORT),y)
        EXTRA_CFLAGS += -DMULTI_APCLI_SUPPORT
    endif
    ap_objs += $(SRC_DIR)/ap/ap_apcli.o\
            $(SRC_DIR)/ap/ap_apcli_inf.o\
            $(SRC_DIR)/ap/apcli_assoc.o\
            $(SRC_DIR)/ap/apcli_auth.o\
            $(SRC_DIR)/ap/apcli_ctrl.o\
            $(SRC_DIR)/ap/apcli_sync.o\
            $(SRC_DIR)/common/cmm_mat.o\
            $(SRC_DIR)/common/cmm_mat_iparp.o\
            $(SRC_DIR)/common/cmm_mat_pppoe.o\
            $(SRC_DIR)/common/cmm_mat_ipv6.o

    ifeq ($(CONFIG_MT_MAC_REPEATER_SUPPORT),y)
        EXTRA_CFLAGS += -DMAC_REPEATER_SUPPORT

        ap_objs += $(SRC_DIR)/ap/ap_repeater.o
    endif
endif

ifeq ($(CONFIG_MT_IDS_SUPPORT),y)
    EXTRA_CFLAGS += -DIDS_SUPPORT

    ap_objs += $(SRC_DIR)/ap/ap_ids.o
endif

ifeq ($(CONFIG_MT_NINTENDO_AP),y)
    EXTRA_CFLAGS += -DNINTENDO_AP

    ap_objs += $(SRC_DIR)/ap/ap_nintendo.o
endif

ifeq ($(CONFIG_MT_COC_SUPPORT),y)
    EXTRA_CFLAGS += -DGREENAP_SUPPORT -DCOC_SUPPORT

    ap_objs += $(SRC_DIR)/hw_ctrl/greenap.o
endif

ifeq ($(CONFIG_MT_ATE_SUPPORT),y)
    EXTRA_CFLAGS += -DCONFIG_ATE -DCONFIG_QA -DCONFIG_RT2880_ATE_CMD_NEW
endif

ifeq ($(CONFIG_MT_RTMP_FLASH_SUPPORT),y)
    EXTRA_CFLAGS += -DRTMP_FLASH_SUPPORT
endif

ifeq ($(CONFIG_MT_TXBF_SUPPORT),y)
    EXTRA_CFLAGS += -DMCS_LUT_SUPPORT -DTXBF_SUPPORT -DVHT_TXBF_SUPPORT
    ap_objs += $(SRC_DIR)/common/cmm_txbf.o\
                    $(SRC_DIR)/common/cmm_txbf_cal.o
endif

ifeq ($(CONFIG_MT_DFS_SUPPORT),y)
    EXTRA_CFLAGS += -DDFS_SUPPORT
    EXTRA_CFLAGS += -DDFS_ATP_SUPPORT
    ap_objs += $(SRC_DIR)/common/cmm_dfs.o
endif

ifeq ($(CONFIG_RT2860V2_AP_CARRIER),y)
    EXTRA_CFLAGS += -DCARRIER_DETECTION_SUPPORT

    ap_objs += $(SRC_DIR)/common/cmm_cs.o
endif

ifeq ($(CONFIG_MT_MCAST_RATE_SPECIFIC),y)
    EXTRA_CFLAGS += -DMCAST_RATE_SPECIFIC
endif

#ifeq ($(CONFIG_RT2860V2_AP_MESH),y)
#EXTRA_CFLAGS += -DMESH_SUPPORT -DINTEL_CMPC
#mesh_objs += $(SRC_DIR)/common/mesh_bmpkt.o\
#                    $(SRC_DIR)/common/mesh_ctrl.o\
#                    $(SRC_DIR)/common/mesh_link_mng.o\
#                    $(SRC_DIR)/common/mesh_sanity.o\
#                    $(SRC_DIR)/common/mesh_tlv.o\
#                    $(SRC_DIR)/common/mesh.o\
#                    $(SRC_DIR)/common/mesh_inf.o\
#                    $(SRC_DIR)/common/mesh_forwarding.o\
#                    $(SRC_DIR)/common/mesh_path_mng.o 
#endif


########################################################
# Linux system related files
########################################################
os_objs := $(SRC_DIR)/os/linux/rt_proc.o\
            $(SRC_DIR)/os/linux/rt_linux.o\
            $(SRC_DIR)/os/linux/rt_profile.o\
            $(SRC_DIR)/os/linux/rt_txrx_hook.o\
            $(SRC_DIR)/os/linux/rt_main_dev.o

ifeq ($(CONFIG_MT_MULTI_INF_SUPPORT),y)
    EXTRA_CFLAGS += -DMULTI_INF_SUPPORT

    os_objs += $(SRC_DIR)/os/linux/multi_main_dev.o
endif

ifeq ($(CONFIG_MT_WIFI_WORK_QUEUE_BH),y)
    EXTRA_CFLAGS += -DWORKQUEUE_BH
endif

ifeq ($(CONFIG_MT_KTHREAD),y)
    EXTRA_CFLAGS += -DKTHREAD_SUPPORT
endif

ifeq ($(CONFIG_MT_BTCOEX_CONCURRENT),y)
    EXTRA_CFLAGS += -DBTCOEX_CONCURRENT
endif


########################################################
# chip related files
########################################################
#ifeq ($(CONFIG_RALINK_RT2880),y)
#EXTRA_CFLAGS += -DRT2880

#chip_objs += $(RT_WIFI_DIR)/chips/rt2880.o
#endif

#ifeq ($(CONFIG_RT2860V2_AP_TXBF),y)
#EXTRA_CFLAGS += -DTXBF_SUPPORT
#endif

#ifeq ($(CONFIG_RALINK_RT2883),y)
#EXTRA_CFLAGS += -DRT2883 -DDOT11N_SS3_SUPPORT -DA_BAND_SUPPORT -DRTMP_RF_RW_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#            $(RT_WIFI_DIR)/chips/rt2883.o
#endif

#ifeq ($(CONFIG_RALINK_RT3883),y)
#EXTRA_CFLAGS += -DRT3883 -DDOT11N_SS3_SUPPORT -DA_BAND_SUPPORT -DRTMP_RF_RW_SUPPORT
#EXTRA_CFLAGS += -DRTMP_FLASH_SUPPORT -DCONFIG_SWMCU_SUPPORT
#EXTRA_CFLAGS += -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT
#EXTRA_CFLAGS += -DRANGE_EXT_SUPPORT
#EXTRA_CFLAGS += -DSTREAM_MODE_SUPPORT -DFIFO_EXT_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#                    $(RT_WIFI_DIR)/chips/rt3883.o
#                    #$(RT_WIFI_DIR)/ate/chips/rt3883_ate.o

#ifeq ($(CONFIG_TXBF_SUPPORT),y)
#chip_objs += $(RT_WIFI_DIR)/common/cmm_txbf.o\
#                    $(RT_WIFI_DIR)/common/cmm_txbf_cal.o
#endif
#endif

#ifeq ($(CONFIG_RALINK_RT3052),y)
#EXTRA_CFLAGS += -DSPECIFIC_VCORECAL_SUPPORT
#ifeq ($(CONFIG_RALINK_RT3350),y)
#EXTRA_CFLAGS += -DRT3350 -DRT305x -DRTMP_RF_RW_SUPPORT
#else
#EXTRA_CFLAGS += -DRT3052 -DRT305x -DRTMP_RF_RW_SUPPORT
#endif

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#                    $(RT_WIFI_DIR)/chips/rt305x.o
#endif

#ifeq ($(CONFIG_RALINK_RT3352),y)
#EXTRA_CFLAGS += -DRT3352 -DRT305x -DRTMP_RF_RW_SUPPORT
#EXTRA_CFLAGS += -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#                    $(RT_WIFI_DIR)/chips/rt305x.o\
#                    $(RT_WIFI_DIR)/chips/rt3352.o
#endif

#ifeq ($(CONFIG_RT3x52),y)
#EXTRA_CFLAGS += -DRT3052 -DRT3352 -DRT305x -DRTMP_RF_RW_SUPPORT -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#                    $(RT_WIFI_DIR)/chips/rt305x.o\
#                    $(RT_WIFI_DIR)/chips/rt3352.o
#endif

#ifeq ($(CONFIG_RALINK_RT5350),y)
#EXTRA_CFLAGS += -DRT5350 -DRT305x -DRTMP_RF_RW_SUPPORT
#EXTRA_CFLAGS += -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#                    $(RT_WIFI_DIR)/chips/rt305x.o\
#                    $(RT_WIFI_DIR)/chips/rt5350.o
#endif

#ifeq ($(CONFIG_RALINK_RT6352),y)
#EXTRA_CFLAGS += -DRT6352 -DRTMP_RF_RW_SUPPORT -DRTMP_FLASH_SUPPORT -DCONFIG_SWMCU_SUPPORT
#EXTRA_CFLAGS += -DRTMP_RBUS_SUPPORT -DRTMP_MAC_PCI
#EXTRA_CFLAGS += -DRTMP_MAC -DRTMP_BBP -DRLT_RF
#EXTRA_CFLAGS += -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT
#EXTRA_CFLAGS += -DRESOURCE_PRE_ALLOC
#EXTRA_CFLAGS += -DENHANCED_STAT_DISPLAY
#EXTRA_CFLAGS += -DLOFT_IQ_CAL_SUPPORT
#EXTRA_CFLAGS += -DRTMP_TEMPERATURE_CALIBRATION
#EXTRA_CFLAGS += -DRELEASE_EXCLUDE
#EXTRA_CFLAGS += -DENHANCE_NEW_MBSSID_MODE
#EXTRA_CFLAGS += -DSPECIFIC_TX_POWER_SUPPORT
#EXTRA_CFLAGS += -DFIFO_EXT_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#		$(RT_WIFI_DIR)/phy/rlt_rf.o\
#            $(RT_WIFI_DIR)/phy/rtmp_phy.o\
#            $(RT_WIFI_DIR)/chips/rt6352.o\
#            $(RT_WIFI_DIR)/common/cmm_rf_cal.o\
#            $(RT_WIFI_DIR)/common/cmm_loft_cal.o
#            #$(RT_WIFI_DIR)/ate/chips/rt6352_ate.o
#endif

#ifeq ($(CONFIG_RALINK_MT7610E),y)
#EXTRA_CFLAGS += -DRT65xx -DMT76x0 -DMT7610 
#EXTRA_CFLAGS += -DRLT_MAC -DRLT_BBP -DRLT_RF -DRTMP_RF_RW_SUPPORT
#EXTRA_CFLAGS += -DRTMP_MAC_PCI -DRTMP_PCI_SUPPORT -DRX_DMA_SCATTER -DCONFIG_ANDES_SUPPORT -DRTMP_EFUSE_SUPPORT
#EXTRA_CFLAGS += -DA_BAND_SUPPORT -DSPECIFIC_BCN_BUF_SUPPORT -DNEW_MBSSID_MODE -DENHANCE_NEW_MBSSID_MODE
#EXTRA_CFLAGS += -DVCORECAL_SUPPORT -DMT76x0_TSSI_CAL_COMPENSATION
#EXTRA_CFLAGS += -DRESOURCE_PRE_ALLOC
#EXTRA_CFLAGS += -DENHANCED_STAT_DISPLAY
#EXTRA_CFLAGS += -DRELEASE_EXCLUDE

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#            $(RT_WIFI_DIR)/phy/rlt_rf.o\
#            $(RT_WIFI_DIR)/phy/rlt_phy.o\
#            $(RT_WIFI_DIR)/phy/rtmp_phy.o\
#            $(RT_WIFI_DIR)/chips/mt76x0.o\
#            $(RT_WIFI_DIR)/chips/rt65xx.o\
#            $(RT_WIFI_DIR)/common/ee_efuse.o\
#            $(RT_WIFI_DIR)/common/ee_prom.o\
#            $(RT_WIFI_DIR)/mcu/mcu_and.o\
#            $(RT_WIFI_DIR)/common/cmm_mac_pci.o\
#            $(RT_WIFI_DIR)/common/cmm_data_pci.o\
#            $(RT_WIFI_DIR)/os/linux/pci_main_dev.o\
#            $(RT_WIFI_DIR)/os/linux/rt_rbus_pci_drv.o
#            #$(RT_WIFI_DIR)/ate/chips/rt6590_ate.o
#endif

#ifeq ($(CONFIG_RALINK_MT7612E),y)
#EXTRA_CFLAGS += -DRT65xx -DMT76x2 -DMT7612
#EXTRA_CFLAGS += -DRLT_MAC -DRLT_BBP -DMT_RF -DRTMP_RF_RW_SUPPORT
#EXTRA_CFLAGS += -DRTMP_MAC_PCI -DRTMP_PCI_SUPPORT -DRX_DMA_SCATTER -DCONFIG_ANDES_SUPPORT -DRTMP_EFUSE_SUPPORT
#EXTRA_CFLAGS += -DA_BAND_SUPPORT -DSPECIFIC_BCN_BUF_SUPPORT -DNEW_MBSSID_MODE -DENHANCE_NEW_MBSSID_MODE
#EXTRA_CFLAGS += -DRESOURCE_PRE_ALLOC -DRTMP_EFUSE_SUPPORT -DCONFIG_FPGA_MODE
#EXTRA_CFLAGS += -DENHANCED_STAT_DISPLAY -DNEW_RATE_ADAPT_SUPPORT -DDYNAMIC_VGA_SUPPORT
#EXTRA_CFLAGS += -DRELEASE_EXCLUDE -DFIFO_EXT_SUPPORT
#EXTRA_CFLAGS += -DMCS_LUT_SUPPORT -DPEER_DELBA_TX_ADAPT 
#EXTRA_CFLAGS += -DSPECIFIC_TX_POWER_SUPPORT

#chip_objs += $(RT_WIFI_DIR)/phy/rt_rf.o\
#            $(RT_WIFI_DIR)/phy/mt_rf.o\
#            $(RT_WIFI_DIR)/phy/rlt_phy.o\
#            $(RT_WIFI_DIR)/phy/rtmp_phy.o\
#            $(RT_WIFI_DIR)/chips/mt76x2.o\
#            $(RT_WIFI_DIR)/chips/rt65xx.o\
#            $(RT_WIFI_DIR)/common/ee_efuse.o\
#            $(RT_WIFI_DIR)/common/ee_prom.o\
#            $(RT_WIFI_DIR)/mcu/mcu_and.o\
#            $(RT_WIFI_DIR)/common/cmm_mac_pci.o\
#            $(RT_WIFI_DIR)/common/cmm_data_pci.o\
#            $(RT_WIFI_DIR)/os/linux/pci_main_dev.o\
#            $(RT_WIFI_DIR)/os/linux/rt_rbus_pci_drv.o
#endif

ifeq ($(CONFIG_RALINK_MT7603E),y)
EXTRA_CFLAGS += -DMT7603 -DMT_BBP -DMT_RF -DRTMP_RF_RW_SUPPORT -DMT_MAC -DRTMP_MAC_PCI -DRTMP_PCI_SUPPORT -DMCS_LUT_SUPPORT
EXTRA_CFLAGS += -DRTMP_EFUSE_SUPPORT -DCAL_FREE_IC_SUPPORT
EXTRA_CFLAGS += -DRELEASE_EXCLUDE
EXTRA_CFLAGS += -DMT_RF -DRTMP_RF_RW_SUPPORT
EXTRA_CFLAGS += -DRTMP_PCI_SUPPORT -DRX_DMA_SCATTER -DCONFIG_ANDES_SUPPORT -DRTMP_EFUSE_SUPPORT
EXTRA_CFLAGS += -DA_BAND_SUPPORT -DNEW_MBSSID_MODE -DENHANCE_NEW_MBSSID_MODE
EXTRA_CFLAGS += -DRESOURCE_PRE_ALLOC -DRTMP_EFUSE_SUPPORT -DCONFIG_FPGA_MODE
EXTRA_CFLAGS += -DENHANCED_STAT_DISPLAY -DNEW_RATE_ADAPT_SUPPORT
EXTRA_CFLAGS += -DRELEASE_EXCLUDE -DFIFO_EXT_SUPPORT
EXTRA_CFLAGS += -DMCS_LUT_SUPPORT
EXTRA_CFLAGS += -DRTMP_WLAN_HOOK_SUPPORT -DUSE_BMC
EXTRA_CFLAGS +=  -DMT7603_E1 -DMT7603_E2
#Force Load F/W one time
EXTRA_CFLAGS += -DLOAD_FW_ONE_TIME

chip_objs += $(SRC_DIR)/chips/mt7603.o\
		$(SRC_DIR)/hw_ctrl/cmm_asic_mt.o\
		$(SRC_DIR)/hw_ctrl/cmm_chip_mt.o\
		$(SRC_DIR)/hw_ctrl/mt_gpio.o\
		$(SRC_DIR)/mac/mt_mac.o\
		$(SRC_DIR)/mcu/mcu.o\
		$(SRC_DIR)/mcu/andes_core.o\
		$(SRC_DIR)/mcu/andes_mt.o\
    $(SRC_DIR)/phy/mt_rf.o\
    $(SRC_DIR)/phy/rf.o\
    $(SRC_DIR)/phy/mt_phy.o\
    $(SRC_DIR)/common/cmm_mac_pci.o\
    $(SRC_DIR)/common/cmm_data_pci.o\
    $(SRC_DIR)/os/linux/pci_main_dev.o\
    $(SRC_DIR)/common/ee_efuse.o\
    $(SRC_DIR)/common/ee_prom.o\
    $(SRC_DIR)/os/linux/rt_rbus_pci_drv.o\
    $(SRC_DIR)/hif/hif_pci.o
endif

#ifeq ($(CONFIG_RALINK_MT7620),y)
#EXTRA_CFLAGS += -DRT6352 \
#				-DRTMP_RF_RW_SUPPORT \
#				-DRF_BANK -DRTMP_FLASH_SUPPORT \
#				-DCONFIG_SWMCU_SUPPORT \
#				-DRTMP_MAC_PCI \
#				-DRTMP_BBP \
#				-DRTMP_RBUS_SUPPORT
#EXTRA_CFLAGS += -DSPECIFIC_BCN_BUF_SUPPORT -DVCORECAL_SUPPORT
#EXTRA_CFLAGS += -DRESOURCE_PRE_ALLOC
#EXTRA_CFLAGS += -DENHANCED_STAT_DISPLAY
#EXTRA_CFLAGS += -DLOFT_IQ_CAL_SUPPORT
#EXTRA_CFLAGS += -DRTMP_TEMPERATURE_CALIBRATION
#EXTRA_CFLAGS += -DDYNAMIC_VGA_SUPPORT
#EXTRA_CFLAGS += -DMCS_LUT_SUPPORT -DPEER_DELBA_TX_ADAPT
#chip_objs += $(RT_WIFI_DIR)/os/linux/rbus_main_dev.o\
#			$(RT_WIFI_DIR)/phy/rf.o\
#			$(RT_WIFI_DIR)/phy/rt_rf.o\
#	        $(RT_WIFI_DIR)/common/cmm_rf_cal.o\
#			$(RT_WIFI_DIR)/phy/rtmp_phy.o\
#	        $(RT_WIFI_DIR)/hw_ctrl/cmm_asic.o\
#	        $(RT_WIFI_DIR)/hif/hif_pci.o\
#	        $(RT_WIFI_DIR)/mcu/mcu.o\
#	        $(RT_WIFI_DIR)/mac/rtmp_mac.o\
#	        $(RT_WIFI_DIR)/common/ee_prom.o\
#	        $(RT_WIFI_DIR)/common/ral_ps.o\
#	        $(RT_WIFI_DIR)/common/rtmp_swmcu.o\
#	        $(RT_WIFI_DIR)/common/cmm_loft_cal.o\
#			$(RT_WIFI_DIR)/chips/rt6352.o
## to check ##
#EXTRA_CFLAGS += -DRLT_RF
#chip_objs += $(RT_WIFI_DIR)/phy/rlt_rf.o
#endif

#
# Root 
#

ifeq ($(CONFIG_SUPPORT_OPENWRT),y)
obj-m += $(DRV_NAME).o
else
obj-$(CONFIG_MT_AP_SUPPORT) += $(DRV_NAME).o
endif

$(DRV_NAME)-objs += $(ap_objs) $(cmm_objs) $(asic_objs) $(chip_objs) $(rate_objs)\
                    $(spec_objs) $(func_objs) $(os_objs)

$(DRV_NAME)-objs += $(SRC_DIR)/common/eeprom.o\
                    $(SRC_DIR)/common/ee_flash.o

$(DRV_NAME)-objs += $(SRC_DIR)/common/cmm_mac_pci.o
$(DRV_NAME)-objs += $(SRC_DIR)/common/cmm_data_pci.o

$(DRV_NAME)-objs += $(SRC_DIR)/os/linux/rt_pci_rbus.o\
                    $(SRC_DIR)/os/linux/rt_rbus_pci_drv.o\
                    $(SRC_DIR)/os/linux/rt_rbus_pci_util.o\
                    #$(SRC_DIR)/os/linux/rbus_main_dev.o

ifeq ($(CONFIG_MT_ATE_SUPPORT),y)
$(DRV_NAME)-objs += $(SRC_DIR)/ate/ate_agent.o\
                    $(SRC_DIR)/ate/qa_agent.o\
                    $(SRC_DIR)/ate/mt_mac/mt_ate.o
endif

###################
#  CFLAGS
##################
EXTRA_CFLAGS += -DAGGREGATION_SUPPORT -DPIGGYBACK_SUPPORT -DWMM_SUPPORT  -DLINUX \
               -Wall -Wstrict-prototypes -Wno-trigraphs
#-DDBG_DIAGNOSE -DDBG_RX_MCS -DDBG_TX_MCS

EXTRA_CFLAGS += -DCONFIG_AP_SUPPORT -DSCAN_SUPPORT -DAP_SCAN_SUPPORT
EXTRA_CFLAGS += -DDOT11_N_SUPPORT -DSTATS_COUNT_SUPPORT -DIAPP_SUPPORT -DDOT1X_SUPPORT
#EXTRA_CFLAGS += -DRALINK_ATE -DRALINK_QA -DCONFIG_RT2880_ATE_CMD_NEW
EXTRA_CFLAGS += -DCONFIG_RA_NAT_NONE

#provide busy time statistics for every TBTT */
#EXTRA_CFLAGS += -DQLOAD_FUNC_BUSY_TIME_STATS 

# provide busy time alarm mechanism 
# use the function to avoid to locate in some noise environments 
#EXTRA_CFLAGS += -DQLOAD_FUNC_BUSY_TIME_ALARM

ifeq ($(CONFIG_RT2860V2_AUTO_CH_SELECT_ENCANCE),y)
EXTRA_CFLAGS += -DAUTO_CH_SELECT_ENHANCE
endif

ifeq ($(CONFIG_RT2860V2_SNMP),y)
EXTRA_CFLAGS += -DSNMP_SUPPORT
endif

ifeq ($(CONFIG_RT2860V2_AP_32B_DESC),y)
EXTRA_CFLAGS += -DDESC_32B_SUPPORT
endif

ifeq ($(CONFIG_RT2860V2_HW_ANTENNA_DIVERSITY),y)
EXTRA_CFLAGS += -DHW_ANTENNA_DIVERSITY_SUPPORT
endif

ifeq ($(CONFIG_RT2860V2_EXT_CHANNEL_LIST),y)
EXTRA_CFLAGS += -DEXT_BUILD_CHANNEL_LIST
endif

ifeq ($(CONFIG_MT_MEMORY_OPTIMIZATION),y)
EXTRA_CFLAGS += -DMEMORY_OPTIMIZATION
else
EXTRA_CFLAGS += -DDBG
endif

ifeq ($(CONFIG_MT_RTMP_INTERNAL_TX_ALC),y)
EXTRA_CFLAGS += -DRTMP_INTERNAL_TX_ALC
endif

EXTRA_CFLAGS += -DIP_ASSEMBLY
#ifeq ($(CONFIG_RT2860V2_AP_RTMP_TEMPERATURE_CALIBRATION),y)
#ifeq ($(CONFIG_RALINK_MT7620),y)
#EXTRA_CFLAGS   += -DRTMP_TEMPERATURE_CALIBRATION
#endif
#endif

MODULE_FLAGS=$(EXTRA_CFLAGS)
export MODULE_FLAGS
obj-m+=$(SRC_DIR)/tools/plug_in/

ifeq ($(CONFIG_MULTI_CORE_SUPPORT),y)
EXTRA_CFLAGS += -DMULTI_CORE_SUPPORT
endif
